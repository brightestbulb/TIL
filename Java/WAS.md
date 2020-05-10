## JAVA로 MINI WAS 만들기

**작성중**

```java
public class WASMain {

    public static void main(String[] args){
        ServerSocket listener = null;

        try{
            listener = new ServerSocket(8080);
            System.out.println("client를 기다린다.");

            while(true){
                Socket client = listener.accept();  // 블로킹 메소드
                System.out.println(client);

                new Thread(() -> {  // 블로킹되는 문제를 막기위해 thread 사용
                    try{
                        handleSocket(client);
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

    private static void handleSocket(Socket client) throws IOException {
        InputStream is = client.getInputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(is));

        String line = "";
        while((line = br.readLine()) != null){
            System.out.println(line);
        }

        is.close();
        client.close();
    }
}
```