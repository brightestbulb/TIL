## IP 주소와 Port

한대의 컴퓨터에서는 다양한 서버 프로그램들이 실행될 수 있다.   
웹 서버, 데이터 베이스 관리 시스템(DBMS), FTP 서버 등이 하나의 IP 주소를 갖는 컴퓨터에서 동시에 실행될 수 있다.   
이 때, 클라이언트는 어떤 서버와 통신해야 할지 결정해야 한다. IP는 컴퓨터의 네트워크 어댑터까지만 갈 수 있는 정보이기 때문에 
컴퓨터 내에서 실행하는 서버를 선택하기 위해서는 추가적인 정보가 필요하다.   
이 정보가 **포트(Port)** 번호이다.   
서버는 시작할 때 고정적인 포트 번호를 가지고 실행하는데, 이것을 **포트 바인딩** 이라고 한다.   
기본적으로 웹 서버는 80번과 바인딩, FTP 서버는 21번과 바인딩한다.   
클라이언트가 웹 서버에 연결하려면 80번으로 연결 요청을 해야 한다.   


## InetAddress로 IP 주소 얻기
자바는 IP 주소를 java.net.InetAddress 객체로 표현한다.   
InetAddress는 로컬 컴퓨터의 IP 주소뿐만 아니라 도메인 이름을 DNS에서 검색한 후 IP 주소를 가져오는 기능을 제공한다.   
```java
InetAddress ia = InetAddress.getLocalHost();  // 로컬 컴퓨터의 InetAddress 얻기
```

// 자세한 내용은 책을 참고하자. (p.1055~)


## TCP 네트워킹
TCP는 연결 지향적 프로토콜이다.   
연결 지향 프로토콜이란 클라이언트와 서버가 연결된 상태에서 데이터를 주고받는 프로토콜이다.    
클라이언트가 연결 요청을 하고, 서버가 연결을 수락하면 통신 선로가 고정되고, 모든 데이터는 고정된 통신 선로를 통해서 순차적으로 전달된다.   
그렇기 때문에 TCP는 데이터를 정확하고 안정적으로 전달한다.   
TCP의 단점은 데이터를 보내기 전에 반드시 연결이 형성되야 한다.   


### ServerSocket과 Socket의 용도
TCP 서버의 역할은 두가지이다.   
첫째, 클라이언트가 연결 요청을 해오면 연결을 수락하는 것 **(ServerSocket 클래스)**   
둘째, 연결된 클라이언트와 통신하는 것이다. **(Socket 클래스)**   
클라이언트가 연결을 요청해오면 **ServerSocket**은 연결을 수락하고 통신용 **Socket**을 만든다.   
서버는 클라이언트가 접속할 포트를 갖고 있어야 하는데, 이 포트를 **바인딩 포트** 라고 한다.   
서버는 고정된 포트 번호에 바인딩해서 실행하므로, ServerSocket을 생성할 때 포트 번호 하나를 지정해야 한다.   
서버가 실행되면 클라이언트는 서버의 IP 주소와 바인딩 포트 번호로 Socket을 생성해서 연결 요청을 할 수 있다.   
ServerSocket은 클라이언트가 연결 요청을 해오면 **accept()** 메소드로 연결 수락을 하고 통신용 Socket을 생성한다.   
그러면 클라이언트와 서버는 각각 **Socket**을 이용해서 데이터를 주고받는다.   

### ServerSocket 생성과 연결 수락

```java
ServerSocket serverSocket = new ServerSocket(5001);  //5001 포트에 바인딩하는 ServerSocket 생성
```
**SererSocket** 은 클라이언트 연결 수락을 위해 accept() 메소드를 실행해야 한다.   
accept() 는 클라이언트가 연결 요청하기 전까지 블로킹(스레드가 대기 상태) 된다.   

클라이언트가 연결을 요청하면 accept()는 클라이언트와 통신할 Socket을 만들고 리턴한다. (= 연결 수락)   
만약 accept()에서 블로깅 되어 있을 때 ServerSocket을 닫기 위해 close() 메소드를 호출하면 SocketException이 발생한다.   
그렇기 떄문에 예외 처리가 필요하다.   

다음 코드는 반복적으로 accept() 메소드를 호출해서 다중 클라이언트 연결을 수락하는 가장 기본적인 코드이다.
```java
public class ServerSample {
    public static void main(String[] args){
        ServerSocket serverSocket = null;
        try{
            serverSocket = new ServerSocket();
            serverSocket.bind(new InetSocketAddress("localhost", 5001));  // InetSocketAddress로 특정 IP접속 연결 수락
            
            while(true){
                System.out.println("연결 기다림");
                Socket socket = serverSocket.accept();   // 클라이언트 연결 수락
                InetSocketAddress isa = (InetSocketAddress) socket.getRemoteSocketAddress();
                System.out.println("연결 수락, IP : " + isa.getHostName());  // getPort() : 클라이언트 포트 번호 리턴
            }
            
        }catch(Exception e){ }
    
        if(!serverSocket.isClosed()){    // ServerSocket이 닫혀있지 않으면 
            try{
                serverSocket.close();    // ServerSocket 닫기
            }catch(IOException e1){ }
        }
    }
}
```

### Socket 생성과 연결 요청
클라이언트가 서버에 연결 요청을 하려면 java.net.Socket을 이용해야 한다.   
Socket 객체를 생성함과 동시에 연결 요청을 하려면 생성자의 매개값으로 서버의 IP 주소와 바인딩 포트번호를 넣는다.

다음은 local PC의 5001 포트에 연결 요청하는 코드이다.
```java
try{
    Socket socket = new Socket("localhost", 5001);  // 방법 1
    Socket socket = new Socket(new InetSocketAdress("localhost", 5001)); // 방법 2
}catch(UnknownHostException e){
    // IP 표기 방법이 잘못된 경우
}catch(IOException e){
    // 해당 포트의 서버에 연결할 수 없는 경우
}
```    
외부 서버에 접속하려면 localhost 대신 정확한 IP를 입력하면 된다.   
도메인 이름을 넣으려면 InetSocketAddress 객체를 사용한다.   

다음 예제는 localhost 5001 포트로 연결을 요청하는 코드이다.    
connect() 메소드가 정상적으로 리턴하면 연결이 성공한 것이다.
```java
public class ClientSample {
    public static void main(String[] args){
        Socket socket = null;
        try{
            socket = new Socket();  // Socket 생성
            System.out.println("연결 요청");
            socket.connect(new InetSocketAddress("localhost", 5001));  // 연결 요청
            System.out.println("연결 성공");
        }catch(Exception e){
            try{
                socket.close();  //연결 끊기
            }catch(IOException e1){ }
        }
    }
}
```

ServerSample 클래스를 먼저 실행하고, ClientSample 클래스를 실행한다.

ServerSample 실행 결과
```java
연결 기다림
연결 수락, IP : localhost
연결 기다림
```

ClientSample 실행 결과
```java
연결 요청
연결 성공
```
  
### Socket 데이터 통신
클라이언트가 연결 요청(connect()) 하고 서버가 연결 수락(accept()) 했다면,   
양쪽의 Socket 객체로 부터 각각 입력 스트림(InputStream)과 출력 스트림(OutputStream)을 얻을 수 있다.
```java
[프로그램]   (OutputStream)  -------->  (InputStream)   [서버]
(Socket)  (InputStream)   <-------- (OutputStream)  (Socket)
```

다음은 Socket으로 부터 InputStream, OutputStream을 얻는 코드이다.

```java
// 입력 스트림 얻기
InputStream is = socket.getInputStream();

// 출력 스트림 얻기
outputStream os = socket.getOutputStream();
```
상대방에게 데이터를 보내기 위해서는 보낼 데이터를 byte[] 배열로 생성하고,   
이것을 매개값으로 해서 OutputStream의 write() 메소드를 호출하면 된다.   
다음은 문자열을 UTF-8로 인코딩한 바이트 배열을 얻어내고, write() 메소드로 전송한다.

```java
String data = "보낼 데이터";
byte[] byteArr = data.getBytes("UTF-8");
OutputStream outputStream = socket.getOutputStream();
outputStream.write(byteArr);
outputStream.flush();
```

상대방이 보낸 데이터를 받기 위해서는 받은 데이터를 저장할 byte[]배열을 하나 생성하고,   
이것을 매개값으로 해서 InputStream의 read() 메소드를 호출하면 된다.   
read() 메소드는 읽은 데이터를 byte[] 배열에 저장하고 읽은 바이트 수를 리턴한다.   

```java
byte[] byteArr = new byte[100];
InputStream inputStream = socket.getInputStream();
int readByteCount = intputStream.read(byteArr);
String data = new String(byteArr, 0, readByteCount, "UTF-8");
```

다음 예제는 연결 성공 후, [1] 클라이언트가 먼저 "Hello Server" 를 서버로 보낸다.  
[2] 서버가 이 데이터를 받고, [3] "Hello Client"를 클라이언트로 보내면, [4] 클라이언트가 받는다.   
```java
public class ClientSample {
    public static void main(String[] args){
        Socket socket = null;
        try{
            socket = new Socket();  // Socket 생성
            System.out.println("연결 요청");
            socket.connect(new InetSocketAddress("localhost", 5001));  // 연결 요청
            System.out.println("연결 성공");
        
            byte[] bytes = null;
            String message = null;
            
            // [1]
            OputputStream os = socket.getOutputStream();
            message = "Hello Server";
            bytes = message.getBytes("UTF-8");
            os.write(bytes);
            os.flush();
            System.out.println("데이터 보내기 성공");

            // [4]
            InputStream is = socket.getInputStream();
            bytes = new byte[100];
            int readByteCount = is.read(bytes);
            message = new String(bytes, 0, readByteCount, "UTF-8");
            System.out.println("데이터 받기 성공");
        }catch(Exception e){
            try{
                socket.close();  //연결 끊기
            }catch(IOException e1){ }
        }
    }
}
```

```java
public class ServerSample {
    public static void main(String[] args){
        ServerSocket serverSocket = null;
        try{
            serverSocket = new ServerSocket();
            serverSocket.bind(new InetSocketAddress("localhost", 5001));  // InetSocketAddress로 특정 IP접속 연결 수락
            
            while(true){
                System.out.println("연결 기다림");
                Socket socket = serverSocket.accept();   // 클라이언트 연결 수락
                InetSocketAddress isa = (InetSocketAddress) socket.getRemoteSocketAddress();
                System.out.println("연결 수락, IP : " + isa.getHostName());  // getPort() : 클라이언트 포트 번호 리턴

                byte[] bytes = null;
                String message = null;
                
                // [2]
                InputStream is = socket.getInputStream();
                bytes = new byte[100];
                int readByteCount = is.read(bytes);
                message = new String(bytes, 0, readByteCount, "UTF-8");
                System.out.println("데이터 받기 성공 : " + message);

                // [3]
                OutputStream os = socket.getOutputStream();
                message = "Hello Client";
                bytes = message.getBytes("UTF-8");
                os.write(bytes);
                os.flush();
                System.out.println("데이터 보내기 성공");

                is.close();
                os.close();
                socket.close();
            }
            
        }catch(Exception e){ }
    
        if(!serverSocket.isClosed()){    // ServerSocket이 닫혀있지 않으면 
            try{
                serverSocket.close();    // ServerSocket 닫기
            }catch(IOException e1){ }
        }
    }
}
```

ServerSample 클래스를 먼저 실행하고, ClientSample 클래스를 실행한다.

ClientSample 실행 결과
```java
연결 요청
연결 성공
데이터 보내기 성공
데이터 받기 성공 : hello client
```

ServerSample 실행 결과
```java
연결 기다림
연결 수락, IP : localhost
데이터 받기 성공 : Hello Server
데이터 보내기 성공
연결 기다림 
```

데이터를 받기 위해 InputStream의 read() 메소드를 호출하면 상대방이 데이터를 보내기 전까지는 블로킹 되는데,    
read() 가 블로킹 해제되고 리턴되는 경우는 다음 세 가지이다.   

**블로킹이 해제되는 경우**   
1. 상대방이 데이터를 보냄 냄                  ->   리턴값 ( 읽은 바이트 수 )
2. 상대방이 정상적으로 Socket의 close() 호출  ->   리턴값 ( -1 )
3. 상대방이 비정상적으로 종료                 ->   리턴값 ( IOException 발생 ) 

상대방이 정상적으로 Socket의 close()를 호출하고 연결을 끊었을 경우와 비정상적으로 종료했을 경우,   
모두 예외 처리를 해서 이쪽도 Socket을 닫기 위해 close()를 호출해야 한다.

```java
try{
    // 1. 상대방이 비정상적으로 종료했을 경우 IOException 발생 
    int readByteCount = inputStream.read(byteArr);

    // 2. 상대방이 정상적으로 Socket의 close()를 호출했을 경우
    if(readByteCount == -1){
        throw new IOException();  // 강제로 IOException 발생시킴
    }

    ...    
}catch(Exception e){
    try{
        socket.close();
    }catch(Exception e2){ }
}
``` 

출처 : 이것이 자바다 - 신용권  
