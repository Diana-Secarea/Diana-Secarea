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
