public class TCPClient {

    public static void main(String[] args) throws Exception{

        Socket socket = new Socket("localhost", 9000);
        OutputStream os = socket.getOutputStream();

        FestivalTicket ft = new FestivalTicket("Beach, please!", 900, new Date(new Date().getTime()+99999999), FestivalTicket.TicketType.two_days);
        ObjectOutputStream oos = new ObjectOutputStream(os);
        oos.writeObject(ft);
        oos.close();
        socket.close();
    }
}
public class TCPServer {
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(9000);
        Socket socket = serverSocket.accept();
        InputStream is = socket.getInputStream();

        ObjectInputStream ois = new ObjectInputStream(is);
        System.out.println(ois.readObject());
        ois.close();
        socket.close();
        serverSocket.close();

    }
}

public class UDPClient {
    public static void main(String[] args) {
        try {
            DatagramSocket clientSocket = new DatagramSocket();
            InetAddress address = InetAddress.getByName("localhost");
            int port = 9999;

            // Create object
            FestivalTicket ticket = new FestivalTicket("Beach, please!", 900, new Date(), FestivalTicket.TicketType.two_days);

            // Serialize to byte[]
            ByteArrayOutputStream byteStream = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(byteStream);
            oos.writeObject(ticket);
            oos.flush();
            byte[] buffer = byteStream.toByteArray();

            // Send packet
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, port);
            clientSocket.send(packet);
        } catch (SocketException e) {
            throw new RuntimeException(e);
        } catch (UnknownHostException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

    }
} public class UDPServer {
    public static void main(String[] args) {
        try {
            DatagramSocket socket = new DatagramSocket(9999);
            byte[] buffer = new byte[1024];
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
            socket.receive(packet);
            // Deserialize object
            ByteArrayInputStream byteStream = new ByteArrayInputStream(packet.getData(), 0, packet.getLength());
            ObjectInputStream ois = new ObjectInputStream(byteStream);
            Object received = ois.readObject();

            System.out.println("Received object: " + received);
            socket.close();

        } catch (SocketException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
} 
    public List<FestivalTicket> getFullPassParticipants(){
        p = festivalTicket -> festivalTicket.getTicketType() == FestivalTicket.TicketType.full_pass;
        return participants.stream().filter(FestivalTicket::isValid).collect(Collectors.toList());
    }

   //collect all valid tickets
    @Override
    public void run() {
        this.validTickets =  participants.stream().filter(FestivalTicket::isValid).collect(Collectors.toList());
    }
/*
    public JSONArray serializeToJson() {
        JSONArray jsonArray = new JSONArray();
        SimpleDateFormat sdf = new SimpleDateFormat("dd.MM.yyyy");

        for (FestivalTicket t : participants) {
            JSONObject jsonObject = new JSONObject();
            jsonObject.put("id", t.getId());
            jsonObject.put("eventName", t.getEventName());
            jsonObject.put("price", t.getPrice());
            jsonObject.put("eventDate", sdf.format(t.getEventDate()));
            jsonObject.put("ticketType", t.getTicketType().toString());

            jsonArray.put(jsonObject);
        }

        return jsonArray;
    }

✅ TCP File Transfer Example
Why TCP?
TCP is reliable and ensures ordered delivery, which makes it ideal for file transfer.

📤 TCP File Sender (Client)
java
Copy
Edit
import java.io.*;
import java.net.Socket;

public class TCPFileSender {
    public static void main(String[] args) throws Exception {
        File file = new File("example.txt");
        Socket socket = new Socket("localhost", 9000);

        byte[] fileBytes = new byte[(int) file.length()];
        FileInputStream fis = new FileInputStream(file);
        BufferedInputStream bis = new BufferedInputStream(fis);
        bis.read(fileBytes, 0, fileBytes.length);

        OutputStream os = socket.getOutputStream();
        os.write(fileBytes, 0, fileBytes.length);
        os.flush();

        bis.close();
        socket.close();
        System.out.println("File sent.");
    }
}
📥 TCP File Receiver (Server)
java
Copy
Edit
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPFileReceiver {
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(9000);
        Socket socket = serverSocket.accept();
        System.out.println("Connected.");

        byte[] buffer = new byte[4096];
        InputStream is = socket.getInputStream();
        FileOutputStream fos = new FileOutputStream("received_tcp.txt");
        BufferedOutputStream bos = new BufferedOutputStream(fos);

        int bytesRead;
        while ((bytesRead = is.read(buffer)) != -1) {
            bos.write(buffer, 0, bytesRead);
        }

        bos.close();
        socket.close();
        serverSocket.close();
        System.out.println("File received.");
    }
}


📤 UDP File Sender (Client)
java
Copy
Edit
import java.io.*;
import java.net.*;

public class UDPFileSender {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket();
        InetAddress address = InetAddress.getByName("localhost");
        File file = new File("example.txt");
        FileInputStream fis = new FileInputStream(file);

        byte[] buffer = new byte[1024];
        int bytesRead;
        while ((bytesRead = fis.read(buffer)) != -1) {
            DatagramPacket packet = new DatagramPacket(buffer, bytesRead, address, 9999);
            socket.send(packet);
            Thread.sleep(10); // to reduce risk of packet loss
        }

        // Send an empty packet as EOF signal
        socket.send(new DatagramPacket(new byte[0], 0, address, 9999));

        fis.close();
        socket.close();
        System.out.println("File sent via UDP.");
    }
}
📥 UDP File Receiver (Server)
java
Copy
Edit
import java.io.*;
import java.net.*;

public class UDPFileReceiver {
    public static void main(String[] args) throws Exception {
        DatagramSocket socket = new DatagramSocket(9999);
        byte[] buffer = new byte[1024];
        FileOutputStream fos = new FileOutputStream("received_udp.txt");

        while (true) {
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
            socket.receive(packet);

            if (packet.getLength() == 0) break; // EOF signal
            fos.write(packet.getData(), 0, packet.getLength());
        }

        fos.close();
        socket.close();
        System.out.println("File received via UDP.");
    }
}
