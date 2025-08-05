import java.util.*;




public class Main
{
    //function for bokoing ticket
    public static void bookTicket(Passenger p)
    {
        TicketBooker booker = new TicketBooker();
        //if no WL is available , means all tickets are filled.. so no tickets available
        if(TicketBooker.availableWaitingList == 0)
        {
            System.out.println("No Tickets Available");
            return;
        }
        //check if preferred berth is available
        if((p.berthPreference.equals("L") && TicketBooker.availableLowerBerths > 0 )||
           (p.berthPreference.equals("M") && TicketBooker.availableMiddleBerths > 0) ||
           (p.berthPreference.equals("U") && TicketBooker.availableUpperBerths > 0))
        {
            System.out.println("Preferred Berth Available");
            if(p.berthPreference.equals("L"))
            {
                System.out.println("Lower Berth Given");
                //call booking function in the TicketBooker class
                booker.bookTicket(p,(TicketBooker.lowerBerthsPositions.get(0)),"L");
                //remove the booked position from available positions and also decrease available seats of that
                // particular type
                TicketBooker.lowerBerthsPositions.remove(0);
                TicketBooker.availableLowerBerths--;
                
                




            }
            else if(p.berthPreference.equals("M"))
            {
                System.out.println("Middle Berth Given");
                //call booking function in the TicketBooker class
                booker.bookTicket(p,(TicketBooker.middleBerthsPositions.get(0)),"M");
                //remove the booked position from available positions and also decrease available seats of that
                // particular type
                TicketBooker.middleBerthsPositions.remove(0);
                TicketBooker.availableMiddleBerths--;




            }
            else if(p.berthPreference.equals("U"))
            {
                System.out.println("Upper Berth Given");
                //call booking function in the TicketBooker class
                booker.bookTicket(p,(TicketBooker.upperBerthsPositions.get(0)),"U");
                //remove the booked position from available positions and also decrease available seats of that
                // particular type
                TicketBooker.upperBerthsPositions.remove(0);
                TicketBooker.availableUpperBerths--;
            }




        }
        //preference not available -> book the available berth
       
        else if(TicketBooker.availableLowerBerths > 0)
        {
            System.out.println("Lower Berth Given");
            //call booking function in the TicketBooker class
            booker.bookTicket(p,(TicketBooker.lowerBerthsPositions.get(0)),"L");
            //remove the booked position from available positions and also decrease available seats of that
            // particular type
            TicketBooker.lowerBerthsPositions.remove(0);
            TicketBooker.availableLowerBerths--;
            




        }
        else if(TicketBooker.availableMiddleBerths > 0)
        {
            System.out.println("Middle Berth Given");
            //call booking function in the TicketBooker class
            booker.bookTicket(p,(TicketBooker.middleBerthsPositions.get(0)),"M");
            //remove the booked position from available positions and also decrease available seats of that
            // particular type
            TicketBooker.middleBerthsPositions.remove(0);
            TicketBooker.availableMiddleBerths--;




        }
        else if(TicketBooker.availableUpperBerths > 0)
        {
            System.out.println("Upper Berth Given");
            //call booking function in the TicketBooker class
            booker.bookTicket(p,(TicketBooker.upperBerthsPositions.get(0)),"U");
            //remove the booked position from available positions and also decrease available seats of that
            // particular type
            TicketBooker.upperBerthsPositions.remove(0);
            TicketBooker.availableUpperBerths--;
            
        }
        // if no berth available go to RAC
        else if(TicketBooker.availableRacTickets > 0)
        {
            System.out.println("RAC available");
            booker.addToRAC(p,(TicketBooker.racPositions.get(0)),"RAC" );
        }
        // if no RAC available go to WL
        else if(TicketBooker.availableWaitingList > 0)
        {
            System.out.println("Added to Waiting List");
            booker.addToWaitingList(p,(TicketBooker.waitingListPositions.get(0)),"WL");
            
        }
        
    }
    //cancel ticket function
    public static void cancelTicket(int id)
    {
        TicketBooker booker = new TicketBooker();
        //check if passenger id is valid
        if(!booker.passengers.containsKey(id))
        {
            System.out.println("Passenger detail Unknown");
        }
        else
            booker.cancelTicket(id);
    }
    public static void main(String[] args)
    {
        Scanner s = new Scanner(System.in);
        boolean loop = true;
        //loop to get choices from user until he stops
        while(loop)
        {
            System.out.println(" 1. Book Ticket \n 2. Cancel Ticket \n 3. Available Tickets \n 4. Booked Tickets \n 5. Exit");
            int choice = s.nextInt();
            switch(choice)
            {
                // book ticket
                case 1:
                {
                    //get details from Passenger
                    System.out.println("Enter Passenger name,age and berth preference (L,M or U)");
                    String name = s.next();
                    int age = s.nextInt();
                    //get berth preference (L,U,M)
                    String berthPreference = s.next();
                    //create a passenger object
                    Passenger p = new Passenger(name,age,berthPreference);
                    //booking
                    bookTicket(p);
                }
                break;
                //cancel ticket
                case 2:
                {
                    //get passenger id to cancel
                    System.out.println("Enter passenger Id to cancel");
                    int id = s.nextInt();
                    cancelTicket(id);
                }
                break;
                //available tickets print
                case 3:
                {
                    TicketBooker booker = new TicketBooker();
                    booker.printAvailable();
                }
                break;
                //occupied tickets print
                case 4:
                {
                    TicketBooker booker = new TicketBooker();
                    booker.printPassengers();
                }
                break;
                //exit
                case 5:
                {
                    loop = false;
                }
                break;
                default:
                break;
            }
        }
    }
}
public class Passenger
{
    static int id = 1;//static variable to give id for every new passenger
    String name;
    int age;
    String berthPreference;// U or L or M
    int passengerId;// id of passenger created automatically
    String alloted;//alloted type (L,U,M,RAC,WL)
    int number;//seat number
    public Passenger(String name,int age,String berthPreference)
    {
        this.name = name;
        this.age = age;
        this.berthPreference = berthPreference;
        this.passengerId = id++;
        alloted = "";
        number = -1;
    }
}
import java.util.*;
public class TicketBooker
{
    //63 berths(upper ,lower , middle)  + ( 18 RAC passengers) 
    //10 waiting list tickets ->21 L, 21 M, 21U , 18RAC, 10WL
    static int availableLowerBerths = 1;//normally 21
    static int availableMiddleBerths = 1;//normally 21
    static int availableUpperBerths = 1;//normally 21
    static int availableRacTickets = 1;//normally 18
    static int availableWaitingList = 1;//normally 10

    static Queue<Integer> waitingList = new LinkedList<>();//queue of WL passengers
    static Queue<Integer> racList =  new LinkedList<>();//queu of RAC passengers
    static List<Integer> bookedTicketList =  new ArrayList<>();//list of bookedticket passengers

    static List<Integer> lowerBerthsPositions = new ArrayList<>(Arrays.asList(1));//normally 1,2,...21
    static List<Integer> middleBerthsPositions = new ArrayList<>(Arrays.asList(1));//normally 1,2,...21
    static List<Integer> upperBerthsPositions = new ArrayList<>(Arrays.asList(1));//normally 1,2,...21
    static List<Integer> racPositions = new ArrayList<>(Arrays.asList(1));//normally 1,2,...18
    static List<Integer> waitingListPositions = new ArrayList<>(Arrays.asList(1));//normally 1,2,...10

    static Map<Integer, Passenger> passengers = new HashMap<>();//map of passenger ids to passengers

    //book ticket
    public void bookTicket(Passenger p, int berthInfo,String allotedBerth)
    {
        //assign the seat number and type of berth(L,U,M)
        p.number = berthInfo;
        p.alloted = allotedBerth;
        // add passenger to the map
        passengers.put(p.passengerId,p);
        //add passenger id to the list of booked tickets
        bookedTicketList.add(p.passengerId);        
        System.out.println("--------------------------Booked Successfully");
    }

    //adding to RAC
    public void addToRAC(Passenger p,int racInfo,String allotedRAC)
    {
        //assign seat number and type(RAC)
        p.number = racInfo;
        p.alloted = allotedRAC;
        // add passenger to the map
        passengers.put(p.passengerId,p);
        //add passenger id to the queue of RAC tickets
        racList.add(p.passengerId);
        //decrease available RAC tickets by 1    
        availableRacTickets--;
        //remove the position that was alloted to the passenger
        racPositions.remove(0);

        System.out.println("--------------------------added to RAC Successfully");
    }

    //adding to WL
    public void addToWaitingList(Passenger p,int waitingListInfo,String allotedWL)
    {
        //assign seat number and type(WL)
        p.number = waitingListInfo; 
        p.alloted = allotedWL;
        // add passenger to the map
        passengers.put(p.passengerId,p);
        //add passenger id to the queue of WL tickets
        waitingList.add(p.passengerId);
        //decrease available WL tickets by 1    
        availableWaitingList--;
        //remove the position that was alloted to the passenger
        waitingListPositions.remove(0);

        System.out.println("-------------------------- added to Waiting List Successfully");
    }

    //cancel ticket
    public void cancelTicket(int passengerId)
    {
        //remove the passenger from the map
        Passenger p = passengers.get(passengerId);
        passengers.remove(Integer.valueOf(passengerId));
        //remove the booked ticket from the list
        bookedTicketList.remove(Integer.valueOf(passengerId));

        //take the booked position which is now free
        int positionBooked = p.number;

        System.out.println("---------------cancelled Successfully");

        //add the free position to the correspoding type of list (either L,M,U)
        if(p.alloted.equals("L")) 
        { 
          availableLowerBerths++;
          lowerBerthsPositions.add(positionBooked);
        }
        else if(p.alloted.equals("M"))
        { 
          availableMiddleBerths++;
          middleBerthsPositions.add(positionBooked);
        }
        else if(p.alloted.equals("U"))
        { 
          availableUpperBerths++;
          upperBerthsPositions.add(positionBooked);
        }

        //check if any RAC is there
        if(racList.size() > 0)
        {
            //take passenger from RAC and increase the free space in RAC list and increase available RAC tickets
            Passenger passengerFromRAC = passengers.get(racList.poll());
            int positionRac = passengerFromRAC.number;
            racPositions.add(positionRac);
            racList.remove(Integer.valueOf(passengerFromRAC.passengerId));
            availableRacTickets++;

            //check if any WL is there
            if(waitingList.size()>0)
            {
                //take the passenger from WL and add them to RAC , increase the free space in waiting list and 
                //increase available WL and decrease available RAC by 1
                Passenger passengerFromWaitingList = passengers.get(waitingList.poll());
                int positionWL = passengerFromWaitingList.number;
                waitingListPositions.add(positionWL);
                waitingList.remove(Integer.valueOf(passengerFromWaitingList.passengerId));

                passengerFromWaitingList.number = racPositions.get(0);
                passengerFromWaitingList.alloted = "RAC";
                racPositions.remove(0);
                racList.add(passengerFromWaitingList.passengerId);
                
                availableWaitingList++;
                availableRacTickets--;
            }
            // now we have a passenger from RAc to whom we can book a ticket, 
            //so book the cancelled ticket to the RAC passenger
            Main.bookTicket(passengerFromRAC);
        }
    
    }

    //print all available seats
    public void printAvailable()
    {
        System.out.println("Available Lower Berths "  + availableLowerBerths);
        System.out.println("Available Middle Berths "  + availableMiddleBerths);
        System.out.println("Available Upper Berths "  + availableUpperBerths);
        System.out.println("Availabel RACs " + availableRacTickets);
        System.out.println("Available Waiting List " + availableWaitingList);
        System.out.println("--------------------------");
    }

    //print all occupied passengers from all types including WL
    public void printPassengers()
    {
        if(passengers.size() == 0)
        {
            System.out.println("No details of passengers");
            return;
        }
        for(Passenger p : passengers.values())
        {
            System.out.println("PASSENGER ID " + p.passengerId );
            System.out.println(" Name " + p.name );
            System.out.println(" Age " + p.age );
            System.out.println(" Status " + p.number + p.alloted);
            System.out.println("--------------------------");
        }
    }
}
