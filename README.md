import java.util.*;

class Room {
    int roomNumber;
    String category; // Standard, Deluxe, Suite
    double price;
    boolean isAvailable;

    public Room(int roomNumber, String category, double price) {
        this.roomNumber = roomNumber;
        this.category = category;
        this.price = price;
        this.isAvailable = true;
    }
}

class Reservation {
    String guestName;
    Room room;
    Date checkIn;
    Date checkOut;

    public Reservation(String guestName, Room room, Date checkIn, Date checkOut) {
        this.guestName = guestName;
        this.room = room;
        this.checkIn = checkIn;
        this.checkOut = checkOut;
    }

    public void printDetails() {
        System.out.println("Reservation for: " + guestName);
        System.out.println("Room No: " + room.roomNumber + " (" + room.category + ")");
        System.out.println("Check-in: " + checkIn);
        System.out.println("Check-out: " + checkOut);
        System.out.println("Total: $" + room.price);
    }
}

public class HotelReservationSystem {
    static List<Room> rooms = new ArrayList<>();
    static List<Reservation> reservations = new ArrayList<>();
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        initRooms();
        int choice;

        do {
            System.out.println("\n--- Hotel Reservation System ---");
            System.out.println("1. View Available Rooms");
            System.out.println("2. Make a Reservation");
            System.out.println("3. View All Reservations");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // clear buffer

            switch (choice) {
                case 1:
                    viewAvailableRooms();
                    break;
                case 2:
                    makeReservation();
                    break;
                case 3:
                    viewReservations();
                    break;
                case 4:
                    System.out.println("Exiting system. Thank you!");
                    break;
                default:
                    System.out.println("Invalid option.");
            }
        } while (choice != 4);
    }

    static void initRooms() {
        rooms.add(new Room(101, "Standard", 100));
        rooms.add(new Room(102, "Standard", 100));
        rooms.add(new Room(201, "Deluxe", 150));
        rooms.add(new Room(202, "Deluxe", 150));
        rooms.add(new Room(301, "Suite", 250));
    }

    static void viewAvailableRooms() {
        System.out.println("\nAvailable Rooms:");
        for (Room room : rooms) {
            if (room.isAvailable) {
                System.out.println("Room " + room.roomNumber + " - " + room.category + " - $" + room.price);
            }
        }
    }

    static void makeReservation() {
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();

        viewAvailableRooms();

        System.out.print("Enter room number to book: ");
        int roomNumber = scanner.nextInt();
        scanner.nextLine();

        Room selectedRoom = null;
        for (Room room : rooms) {
            if (room.roomNumber == roomNumber && room.isAvailable) {
                selectedRoom = room;
                break;
            }
        }

        if (selectedRoom == null) {
            System.out.println("Room not available or invalid.");
            return;
        }

        System.out.println("Simulated payment of $" + selectedRoom.price + " completed.");
        selectedRoom.isAvailable = false;

        Date checkIn = new Date(); // now
        Calendar cal = Calendar.getInstance();
        cal.setTime(checkIn);
        cal.add(Calendar.DATE, 1); // +1 day
        Date checkOut = cal.getTime();

        Reservation res = new Reservation(name, selectedRoom, checkIn, checkOut);
        reservations.add(res);

        System.out.println("\nReservation Successful!");
        res.printDetails();
    }

    static void viewReservations() {
        if (reservations.isEmpty()) {
            System.out.println("No reservations made yet.");
            return;
        }

        System.out.println("\nAll Reservations:");
        for (Reservation res : reservations) {
            res.printDetails();
            System.out.println("-------------------------");
        }
    }
}
