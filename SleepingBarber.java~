import java.util.concurrent.*;
import java.util.Date;
import java.text.*;
import java.util.Scanner;

public class SleepingBarber extends Thread {

  /* PREREQUISITES */


  /* we create the semaphores. First there are no customers and 
   the barber is asleep so we call the constructor with parameter
   0 thus creating semaphores with zero initial permits. 
   Semaphore(1) constructs a binary semaphore, as desired. */
  
    public static Semaphore customers = new Semaphore(0);
    public static Semaphore barber = new Semaphore(0);
    public static Semaphore accessSeats = new Semaphore(1);

  /* we denote that the number of chairs in this barbershop is 5. */

    public static final int CHAIRS = 5;

  /* we create the integer numberOfFreeSeats so that the customers
   can either sit on a free seat or leave the barbershop if there
   are no seats available */

   public static int numberOfFreeSeats = CHAIRS;

   
/* THE CUSTOMER THREAD */

class Customer extends Thread {
  
  /* we create the integer iD which is a unique ID number for every customer
     and a boolean notCut which is used in the Customer waiting loop */
  
  int iD;
  boolean notCut=true;

  /* Constructor for the Customer */
    
  public Customer(int i) {
    iD = i;
  }

  public void run() {   
    
    Date dNow= new Date();
    
    SimpleDateFormat ft = 
      new SimpleDateFormat ("hh:mm:ss a");


    
    while (notCut) {  // as long as the customer is not cut 
      try {
      accessSeats.acquire();  //tries to get access to the chairs
      if (numberOfFreeSeats > 0) {  //if there are any free seats
        System.out.println("Customer " + this.iD + " enters the barber shop at " + ft.format(dNow) + "\n");
        numberOfFreeSeats--;  //sitting down on a chair
        customers.release();  //notify the barber that there is a customer
        accessSeats.release();  // don't need to lock the chairs anymore  
        try {
 barber.acquire();  // now it's this customers turn but we have to wait if the barber is busy
        notCut = false;  // this customer will now leave after the procedure
        this.get_haircut();  //cutting...
        } catch (InterruptedException ex) {}
      }   
      else  {  // there are no free seats
        System.out.println("There are no free seats. Customer " + this.iD + " has left the barber shop\n");
        accessSeats.release();  //release the lock on the seats
        notCut=false; // the customer will leave since there are no spots in the queue left.
      }
     }
      catch (InterruptedException ex) {}
    }
  }

  /* this method will simulate getting a hair-cut */
  
  public void get_haircut(){
    System.out.println("Customer " + this.iD + " is getting their hair cut\n");
    try {
    sleep(5050);
    } catch (InterruptedException ex) {}
  }

}

 
// THE BARBER  


class Barber extends Thread {
  
  int iD2 = 1;
  
  public Barber() {}
  
  public void run() {
    
  
    
    while(true) {  // runs in an infinite loop
      try {
      customers.acquire(); // tries to acquire a customer - if none is available he goes to sleep
      accessSeats.release(); // at this time he has been awaken -> want to modify the number of available seats
        numberOfFreeSeats++; // chair gets free
      barber.release();  // the barber is ready to cut
      accessSeats.release(); // we don't need the lock on the chairs anymore
      this.cutHair();  //cutting...
    } catch (InterruptedException ex) {}
    }
  }

    /* this method will simulate cutting hair */
   
  public void cutHair(){
    
    Date dNow1 = new Date();
    
     SimpleDateFormat ft1 = 
      new SimpleDateFormat ("hh:mm:ss a");
    
    System.out.println("The barber " + this.iD2 + " is cutting hair at " + ft1.format(dNow1) + "\n");
    try {
      sleep(5000); // rate to determine speed of cutting hair
    } catch (InterruptedException ex){ }
  }
}       
  






  /* MAIN METHOD */

  public static void main(String args[]) {
    
    SleepingBarber barberShop = new SleepingBarber();  //Creates a new barbershop
    barberShop.start();  // Begin
  }

  public void run(){   
    
    System.out.println("\n***This code has 1 Barber working today***\n A Customer enters the store every 1 second while the barber takes 5 seconds to cut the next persons hair\n This makes it a 10/20 (50%) success rate\n");
    
    
 
   Barber giovanni = new Barber();  
   giovanni.start();  //Ready for another day of work
    
   
   
   /* This method will create new customers for a while */
    
   for (int i=1; i<21; i++) {
     Customer aCustomer = new Customer(i);
     aCustomer.start();
     try {
       sleep(1100); // Rate to determine fixed intervals of customers 
     } catch(InterruptedException ex) {};
   }
  
  } 
}