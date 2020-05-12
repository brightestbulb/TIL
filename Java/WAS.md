## JAVA로 MINI WAS 만들기

```java
public class WASMain {

    public static void main(String[] args){
        ServerSocket listener = null;

        try{
            listener = new ServerSocket(8080);
            System.out.println("client를 기다린다.");

            while(true){
                Socket socket = listener.accept();  // 블로킹 메소드
                System.out.println(socket);

                new Thread(() -> {  // 블로킹되는 문제를 막기위해 thread 사용
                    try{
                        handleSocket(socket);
                    }catch(Exception ex){
                        ex.printStackTrace();
                    }
                }).start();
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally {
            try{
                listener.close();
            }catch(Exception e){ }
        }
    }

    private static void handleSocket(Socket socket) throws IOException {

        OutputStream os = socket.getOutputStream(); // 클라이언트에게 데이터를 보내기 위한 작업
        PrintWriter pw = new PrintWriter(new OutputStreamWriter(os));

        InputStream is = socket.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(is));

        String line = "";
        HttpRequest request = new HttpRequest();
        line = br.readLine();
        String[] firstLineArgs = line.split(" ");
        request.setMethod(firstLineArgs[0]);
        request.setPath(firstLineArgs[1]);

        while((line = br.readLine()) != null){

            if("".equals(line)){  // 헤더를 읽고 빈줄을 만나면
                break;
            }

            String[] headerArray = line.split(" ");

            if(headerArray[0].startsWith("Host:")){

                request.setHost(headerArray[1].trim());

            }else if(headerArray[0].startsWith("Content-Length:")){

                int length = Integer.parseInt(headerArray[1].trim());
                request.setContentLength(length);

            }else if(headerArray[0].startsWith("User-Agent:")){

                request.setUserAgent(line.substring(12));

            }else if(headerArray[0].startsWith("Content-Type:")){

                request.setContentType(headerArray[1].trim());

            }
        }

        System.out.println(request);

        String baseDir = "/tmp/wasroot";
        String fileName = request.getPath();

        if("/".equals(fileName)){
            fileName = "/index.html";
        }else if(fileName.endsWith(".png")){
            fileName = request.getPath();
        }else{
            fileName = "/error.html";
        }

        fileName = baseDir + fileName;

        String contentType = "text/html; charset=UTF-8";
        if(fileName.endsWith(".png")){
            contentType = "image/png";
        }

        File file = new File(fileName); // java.io.File
        long fileLength = file.length();

        if(file.isFile()){
            pw.println("HTTP/1.1 200 OK");
            pw.println("Content-Type: " + contentType);
            pw.println("Content-Length: " + fileLength);
            pw.println();
        }else{
            pw.println("HTTP/1.1 404 OK");
            pw.println("Content-Type: " + contentType);
            pw.println("Content-Length: " + fileLength);
            pw.println();
        }

        pw.flush();  // 헤더와 빈줄을 char 형식으로 출력

        FileInputStream fis = new FileInputStream(file);
        byte[] buffer = new byte[1024];
        int readCount = 0;
        while((readCount = fis.read(buffer)) != -1){
            os.write(buffer, 0, readCount);
        }

        os.flush();
        os.close();
        is.close();
        socket.close();  // 클라이언트와 접속 close
    }
}

```

```java
public class HttpRequest {

    private String method; //GET, POST, PUT, DELETE
    private String path;
    private String host; // host
    private int contentLength; //Content-Length : body 길이
    private String userAgent; // User-Agent : 브라우저 정보
    private String contentType; // Content-Type : 사용자가 요청한 컨텐츠의 타입

    // getter, setter
}
```

클라이언트 요청 시 출력
```
HttpRequest{
	method='GET', path='/', host='10.10.88.15:8080', contentLength='0', 
	userAgent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36', 	contentType='null'
}


HttpRequest{
	method='GET', path='/hello.png', host='10.10.88.15:8080', contentLength='0', 
	userAgent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36', 	contentType='null'
}
```

출처  : https://lalwr.blogspot.com/2018/03/java-mini-was_24.html