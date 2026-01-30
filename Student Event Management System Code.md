# Code Overview
This document contains all the code for the student event management system. Look out for updates to further expand the system into a full-stack application with database connectivity. 

## Student Class
```java

public class Student {
	
	// Declaring instance variables
	
	private int studentId;
	private String studentName;
	private String studentEmail;
	private String studentPassword;
	private String studentMajor;
	private String studentYear;
	private static int idCounter = 1000;
	
	// Declaring constructor class
	
	public Student(String name, String email, String password, String major, String year)
	{
		studentId = idCounter;
		idCounter++;
		studentName = name;
		studentEmail = email;
		studentPassword = password;
		studentMajor = major;
		studentYear = year;
	}
	
	// Declaring setter and getter methods
	
	public void setEmail(String emailParam)
	{
		studentEmail = emailParam;
	}
	
	public void setPassword(String passwordParam)
	{
		studentPassword = passwordParam;
	}
	
	public void setMajor(String majorParam)
	{
		studentMajor = majorParam;
	}
	
	public void setYear(String yearParam)
	{
		studentYear = yearParam;
	}
	
	public String getName()
	{
		return studentName;
	}
	
	public String getEmail()
	{
		return studentEmail;
	}
	
	public String getPassword()
	{
		return studentPassword;
	}
	
	public String getMajor()
	{
		return studentMajor;
	}
	
	public String getYear()
	{
		return studentYear;
	}
	
	public void setStudentId(int id)
	{
		studentId = id;
	}
	public int getstudentId()
	{
		return studentId;
	}
	
	// Declaring a method to read student info
	
	public String toString()
	{
		    return "Student ID: " + studentId + "\n" +
		           "Name: " + studentName + "\n" +
		           "Email: " + studentEmail + "\n" +
		           "Major: " + studentMajor + "\n" +
		           "Year: " + studentYear;
	}

}

```

## Authentication Manager
```java
import java.util.ArrayList;
import java.io.*;
import java.util.Scanner;

public class AuthenticationManager {

		private ArrayList<Student> studentDatabase;
		private Student currentUser;
		
		// Constructor for class
		public AuthenticationManager() 
		{
			studentDatabase = new ArrayList<Student>();
			currentUser = null;
			loadStudentsFromFile();
		}
		
	
		public void register(String nameParam, String emailParam, String passwordParam, String majorParam, String yearParam)
		{
			
			
			// Check for duplicate email address
			for (int i = 0; i < studentDatabase.size(); i++)
			{
				if(studentDatabase.get(i).getEmail().equals(emailParam))
				{
					System.out.println("Email already exists. Please try logging in.");
					return;
				}
			}
			
			// Check if email ends in @asu.edu
			if(!emailParam.endsWith("@asu.edu"))
			{
				System.out.println("Email address is invalid. Please enter an email address ending in @asu.edu");
				return;
			}
			
			// Checks to see if the password is at least 8 characters
			if(passwordParam.length() < 8)
			{
				System.out.println("Password must be at least 8 characters.");
				return;
			}
			
			// Check if password has a number and a symbol
			boolean hasNumber = false;
			boolean hasSymbol = false;
			
			for (int i = 0; i < passwordParam.length(); i++)
			{
				// Gets the character at each position in the password
				char currentChar = passwordParam.charAt(i);
				
				// Checks to see if the password has a digit
				if (Character.isDigit(currentChar))
				{
					hasNumber = true;
				}
				
				
				// Checks to see if a symbol is found in password
				if(!Character.isLetter(currentChar) && !Character.isDigit(currentChar))
				{
					hasSymbol = true;
				}
				
				
			}
			
			// Error message if no digit is found (Must be outside loop or boolean would change for each character)
			if(!hasNumber)
			{
				System.out.println("Password must contain a number and symbol.");
				return;
			}
			
			// Error message if no symbol is found 
			if(!hasSymbol)
			{
				System.out.println("Password must contain a number and symbol.");
				return;
			}
			
			
			// Create and add new student object to studentDatabase
			
			Student newStudent = new Student(nameParam, emailParam, passwordParam, majorParam, yearParam);
			studentDatabase.add(newStudent);
			saveStudentToFile(newStudent);
			System.out.println("\nRegistration Complete!");
			System.out.println("Please review the following information: \n");
			System.out.println(newStudent);
			
		}
		
		public void login(String emailParam, String passwordParam)
		{
			for (int i = 0; i < studentDatabase.size(); i++)
			{
				if(studentDatabase.get(i).getEmail().equals(emailParam))
				{
					if(studentDatabase.get(i).getPassword().equals(passwordParam))
					{
						System.out.print("\nLogin was successful. Redirecting you to homepage.\n");
						currentUser = studentDatabase.get(i);
						return;
					} 
					else
					{
						System.out.println("Invalid Username or Password. Please try again");
						return;
					}
				}
			}
			
			if(currentUser == null)
			{
				System.out.println("Invalid Username or Password. Please try again.");
				return;
			}
		}
		
		// Ends the program when the user logs out
		public void logout()
		{
			currentUser = null;
			System.out.println("You are now logged out.");
		}
		
		// Checks to see if a student is logged in
		public boolean isLoggedIn()
		{
			if(currentUser != null)
			{
				return true;
			}
			else 
			{
				return false;
			}
		}
		
		// Displays who the currentUser is on the website
		public Student getCurrentUser()
		{
			return currentUser;
		}
		
		// View the profile for the current user
		public void viewProfile()
		{
			if(isLoggedIn())
			{
				System.out.println(currentUser);
			}
			else
			{
				System.out.println("Please login first.");
			}
		}
		
		public void printAllStudents() {
		    System.out.println("\n===== DATABASE DEBUG =====");
		    System.out.println("Total students: " + studentDatabase.size());
		    
		    for (int i = 0; i < studentDatabase.size(); i++) {
		        Student s = studentDatabase.get(i);
		        System.out.println("\nStudent #" + (i + 1));
		        System.out.println("  ID: " + s.getstudentId());
		        System.out.println("  Name: " + s.getName());
		        System.out.println("  Email: '" + s.getEmail() + "'");
		        System.out.println("  Password: '" + s.getPassword() + "'");
		        System.out.println("  Major: " + s.getMajor());
		        System.out.println("  Year: " + s.getYear());
		    }
		    System.out.println("=========================\n");
		}
		
		// Saves the information when a student registers to a text file
		private void saveStudentToFile(Student student)
		{
			try
			{
				FileWriter fw = new FileWriter("student.txt", true);
				PrintWriter pw = new PrintWriter(fw);
				
				pw.println(student.getstudentId() + "," + 
		                   student.getName() + "," + 
		                   student.getEmail() + "," + 
		                   student.getPassword() + "," + 
		                   student.getMajor() + "," + 
		                   student.getYear());
				pw.close();
			} catch (IOException e) {
				System.out.println("Error saving student data: " + e.getMessage());
			}
		}
		
		// Loads the text file containing student information 
		private void loadStudentsFromFile()
		{
			try {
				File file = new File("student.txt");
				
				if(!file.exists()) {
					return;
				}
				
				Scanner fileScanner = new Scanner(file);
				
				while(fileScanner.hasNextLine())
				{
					String line = fileScanner.nextLine();
					String[] parts = line.split(",");
					
					Student student = new Student(parts[1], parts[2], parts[3], parts[4], parts[5]);
					student.setStudentId(Integer.parseInt(parts[0]));
					studentDatabase.add(student);
				}
				
				fileScanner.close();
			} catch (FileNotFoundException e) {
				System.out.println("No existing data file found. Starting fresh.");
			}
			
		}
}

```

## Event Class
```java
 import java.time.LocalDate;
 import java.time.LocalTime;
 
public class Event {
	public static void main(String[] arg)
	{
		LocalDate date = LocalDate.parse("2025-11-30");
		LocalTime time = LocalTime.parse("06:00");
		Event v = new Event("Volleyball", "Come out to the sands!", date, time, "Sand Courts", 12, 0.0, "Upcoming");
		Event b = new Event("Basketball", "Come out to the courts!", date, time, "Sand Courts", 12, 0.0, "Upcoming");
		
		
	}
	
	private int eventId;
	private String title;
	private String description;
	private LocalDate date;
	private LocalTime time;
	private String venue;
	private int capacity;
	private double fee;
	private String status;
	private static int idcounter = 1000;
	
	public Event(String titleParam, String descriptionParam, LocalDate dateParam, LocalTime timeParam, String venueParam, int capacityParam, double feeParam, String statusParam)
	{
		eventId = idcounter;
		idcounter++;
		title = titleParam;
		description = descriptionParam;
		date = dateParam;
		time = timeParam;
		venue = venueParam;
		capacity = capacityParam;
		fee = feeParam;
		status = statusParam;
		
	}
	
	public void setEventId(int id)
	{
		eventId = id;
	}
	
	public int getEventId()
	{
		return eventId;
	}
	
	public void setTitle(String titleParam)
	{
		title = titleParam;
	}
	
	public String getTitle()
	{
		return title;
	}
	
	public void setDescription(String descriptionParam)
	{
		description = descriptionParam;
	}
	
	public String getDescription()
	{
		return description;
	}
	
	public void setDate(LocalDate dateParam)
	{
		date = dateParam;
	}
	
	public LocalDate getDate()
	{
		return date;
	}
	
	public void setTime(LocalTime timeParam)
	{
		time = timeParam;
	}
	
	public LocalTime getTime()
	{
		return time;
	}
	
	public void setVenue(String venueParam)
	{
		venue = venueParam;
	}
	
	public String getVenue()
	{
		return venue;
	}
	
	public void setCapacity(int capacityParam)
	{
		capacity = capacityParam;
	}
	
	public int getCapacity()
	{
		return capacity;
	}
	
	public void setFee(double feeParam)
	{
		fee = feeParam;
	}
	
	public double getFee()
	{
		return fee;
	}
	
	public void setStatus(String statusParam)
	{
		status = statusParam;
	}
	
	public String getStatus()
	{
		return status;
	}
	
	public String toString()
	{
		return	"Event ID: " + eventId + "\n" +
				"Title: " + title + "\n" +
				"Description: " + description + "\n" +
				"Date: " + date + "\n" +
				"Time: " + time + "\n" +
				"Venue: " + venue + "\n" +
				"Capacity: " + capacity + "\n" +
				"Fee: " + fee + "\n" +
				"Status: " + status + "\n";
	}

}

```
## Event Manager
```java
import java.time.LocalDate;
import java.time.LocalTime;
import java.util.ArrayList;
import java.io.*;
import java.util.Scanner;

public class EventManager {

	private ArrayList<Event> eventDatabase;
	
	public EventManager()
	{
		eventDatabase = new ArrayList<Event>();
		loadEventsFromFile();
	}
	
	public void createEvent(String titleParam, String descriptionParam, LocalDate dateParam, LocalTime timeParam, String venueParam, int capacityParam, double feeParam, String statusParam)
	{
		if(titleParam == null || titleParam.trim().isEmpty())
		{
			System.out.println("Title can not be blank.");
		}
		
		if(dateParam == null)
		{
			System.out.println("Date can not be blank.");
		}
		else if(dateParam.isBefore(LocalDate.now()))
		{
			System.out.println("Date cannot be set in the past.");
		}
		
		if(timeParam == null)
		{
			System.out.println("Time can not be blank.");
		}
		
		if(venueParam == null || venueParam.trim().isEmpty())
		{
			System.out.println("Venue can not be blank.");
		}
		
		if(capacityParam <= 0)
		{
			System.out.println("Capacity must be greater than 0");
		}
		
		if(feeParam < 0)
		{
			System.out.println("Fee cannot be negative.");
		}
		
		if(statusParam == null || statusParam.trim().isEmpty())
		{
			System.out.println("Status cannot be empty.");
		}
		
		Event newEvent = new Event(titleParam, descriptionParam, dateParam, timeParam, venueParam, capacityParam, feeParam, statusParam );
		eventDatabase.add(newEvent);
		saveEventToFile(newEvent);
		System.out.println("\nEvent Creation Complete!");
		System.out.println("Please review the following information: \n");
		System.out.println(newEvent);
	}
	
	public ArrayList<Event> getAllEvents()
	{
		return eventDatabase;
	}
	
	public ArrayList<Event> searchByKeyword(String keyword)
	{
		// Creates new arraylist for events matching keyword
		ArrayList<Event> matchingEvents = new ArrayList<Event>();
		
		// Filters through events finding matches with keywords
		for(int i = 0; i < eventDatabase.size(); i++)
		{
			if(eventDatabase.get(i).getTitle().toLowerCase().contains(keyword.toLowerCase()) || eventDatabase.get(i).getDescription().toLowerCase().contains(keyword.toLowerCase())
			|| eventDatabase.get(i).getStatus().toLowerCase().contains(keyword.toLowerCase()) || eventDatabase.get(i).getVenue().toLowerCase().contains(keyword.toLowerCase()))
			{
				matchingEvents.add(eventDatabase.get(i)); //adds events to new array list
			}
		}
		
		// returns list to user 
		return matchingEvents;
	}
	
	private void saveEventToFile(Event event)
	{
		try
		{
			FileWriter fw = new FileWriter("event.txt", true);
			PrintWriter pw = new PrintWriter(fw);
	            
	            pw.println(event.getEventId() + "," +
	                       event.getTitle() + "," +
	                       "\"" + event.getDescription() + "\"," +  // Quotes around description
	                       event.getDate() + "," +
	                       event.getTime() + "," +
	                       event.getVenue() + "," +
	                       event.getCapacity() + "," +
	                       event.getFee() + "," +
	                       event.getStatus());
			pw.close();
		} catch (IOException e) {
			System.out.println("Error saving event data: " + e.getMessage());
		}
	}
	private void loadEventsFromFile() {
	    try {
	    	
	        File file = new File("event.txt");
	        
	        if(!file.exists()) {
	            return;
	        
	        }
	        
	        Scanner fileScanner = new Scanner(file);
	        
	        while(fileScanner.hasNextLine()) {
	            String line = fileScanner.nextLine();
	            String[] parts = line.split(",");
	            
	            // Remove quotes from description
	            String description = parts[2].replace("\"", "");
	            
	            Event event = new Event(
	                parts[1],                           
	                description,                        
	                LocalDate.parse(parts[3]),        
	                LocalTime.parse(parts[4]),        
	                parts[5],                          
	                Integer.parseInt(parts[6]),        
	                Double.parseDouble(parts[7]),      
	                parts[8]                           
	            );
	            event.setEventId(Integer.parseInt(parts[0]));
	            
	            eventDatabase.add(event);
	        }
	        
	        fileScanner.close();
	        
	    } catch (FileNotFoundException e) {
	        System.out.println("No existing data file found. Starting fresh.");
	    } 
	}
}
```

## SEMS Application
```java
import java.time.LocalDate;
import java.time.LocalTime;
import java.util.ArrayList;
import java.util.Scanner;
import java.time.format.DateTimeParseException;

public class SEMSApplication {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Scanner inputDevice = new Scanner(System.in);
		AuthenticationManager authManager = new AuthenticationManager();
		EventManager eventManager = new EventManager();
		
		System.out.println("\n\n================================\n");
		System.out.println("Student Event Management System");
		System.out.println("    Any State University\n");
		System.out.println("================================");

		boolean running = true;
		
		while(running)
		{
			if(!authManager.isLoggedIn())
			{
				displayMainMenu();
				int choice = Integer.parseInt(inputDevice.nextLine());
				
				switch (choice)
				{
					case 1:
						System.out.println("======= LOGIN ========");
						
						System.out.print("Please enter your email: ");
						String email = inputDevice.nextLine();
						
						System.out.print("Please enter your password: ");
						String password = inputDevice.nextLine();
						
						authManager.login(email, password);
						
						break;
					
					case 2:
						System.out.println("======== REGISTER ========");
						System.out.print("\nPlease enter your full name: ");
						String name = inputDevice.nextLine();
						
						System.out.print("Please enter your email(must end with @asu.edu): ");
						String regEmail = inputDevice.nextLine();
						
						System.out.print("Please enter a password(must contain number and symbol): ");
						String regPassword = inputDevice.nextLine();
						
						System.out.print("Please enter your major: ");
						String regMajor = inputDevice.nextLine();
						
						System.out.print("Please enter your year (Freshman/Sophmore/Junior/Senior): ");
						String regYear = inputDevice.nextLine();
						
						authManager.register(name, regEmail, regPassword, regMajor, regYear);
						break;
					
					case 3:
						System.out.println("Goodbye!");
						running = false;
						break;
					
					case 4:
						authManager.printAllStudents();
						break;
					
					default:
						System.out.println("Invalid choice. Please enter a number 1-3.");
						break;
				}
			}
			else
			{
				displayUserMenu();
				int choice = Integer.parseInt(inputDevice.nextLine());
				
				switch (choice)
				{
					case 1:
						authManager.viewProfile();
						break;
						
					case 2:
					    try
					    {
					        
					        System.out.println("\n======== Create Event ========");
							System.out.print("\nPlease enter a title: ");
							String title = inputDevice.nextLine();
							
							System.out.print("Please enter a description: ");
							String description = inputDevice.nextLine();
							
							System.out.print("Please enter a date (YYYY-MM-DD): ");
							LocalDate date = LocalDate.parse(inputDevice.nextLine());
							
							System.out.print("Please enter a time (HH:MM): ");
							LocalTime time = LocalTime.parse(inputDevice.nextLine());
							
							System.out.print("Please enter a venue: ");
							String venue = inputDevice.nextLine();
							
							System.out.print("Please enter a maximum capacity: ");
							int capacity = Integer.parseInt(inputDevice.nextLine());
							
							System.out.print("Please enter a fee(if none, put 0): ");
							double fee = Double.parseDouble(inputDevice.nextLine());
							
							System.out.print("Please enter a status(Upcoming/Active/Past): ");
							String status = inputDevice.nextLine();
							
							eventManager.createEvent(title, description, date, time, venue, capacity, fee, status);
					    }
					    catch (DateTimeParseException e)
					    {
					        System.out.println("Error: Invalid date or time format.");
					        System.out.println("Please use YYYY-MM-DD for date and HH:MM for time.");
					    }
					    catch (NumberFormatException e)
					    {
					        System.out.println("Error: Invalid number format for capacity or fee.");
					    }
					    break;
						
						
					case 3:
						ArrayList<Event> allResults = eventManager.getAllEvents();
						if(allResults.isEmpty())
						{
							System.out.println("\nNo matches found. Redirecting to homepage.");
						}
						
						for (int i = 0; i < allResults.size(); i++)
						{
							
							System.out.println("\n----------------");
							System.out.println(allResults.get(i));
							System.out.println("------------------");
						}
						break;
					
					case 4:
						System.out.print("Please enter keywords to search for: ");
						String keywords = inputDevice.nextLine();
						ArrayList<Event> results = eventManager.searchByKeyword(keywords);
						if(results.isEmpty())
						{
							System.out.println("\nNo matches found. Redirecting to homepage.");
						}
						
						for (int i = 0; i < results.size(); i++)
						{
							System.out.println("\n----------------");
							System.out.println(results.get(i));
							System.out.println("------------------");
						}

						break;
						
					case 5: 
						authManager.logout();
						break;
					
					case 6:
						running = false;
						break;
					
					default:
						System.out.println("Invalid choice. Please enter a number 1-6.");
						break;
						
				}
				
			}
		}
		
	}
	
	public static void displayMainMenu()
	{
		System.out.println("\n====== MAIN MENU =======");
		System.out.println("1. Login");
		System.out.println("2. Register");
		System.out.println("3. Exit");
		System.out.println("===========================");
		System.out.print("Please enter a choice: ");
	}
	
	public static void displayUserMenu()
	{
		System.out.println("\n====== USER MENU =======");
		System.out.println("1. View Profile");
		System.out.println("2. Create Event");
		System.out.println("3. Browse Events");
		System.out.println("4. Search Events");
		System.out.println("5. Logout");
		System.out.println("6. Exit");
		System.out.println("===========================");
		System.out.print("Please enter a choice: ");
	}

}

```
