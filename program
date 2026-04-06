using System;
using System.Collections.Generic;

namespace WorkplaceManagementSystem
{
    public class LogEntry
    {
        public string ID { get; set; }
        public string Name { get; set; }
        public string Branch { get; set; }
        public DateTime ClockIn { get; set; }
        public DateTime ClockOut { get; set; }
        public double HoursWorked { get; set; }
        public string StatusNote { get; set; }
    }

    public abstract class ShiftMetrics
    {
        public abstract double GetTotalTime(DateTime start, DateTime end);
        public abstract string GetRemarks(double hours);
    }

    public class AttendanceProcessor : ShiftMetrics
    {
        private const double Goal = 9.0;

        public override double GetTotalTime(DateTime start, DateTime end)
        {
            return Math.Round((end - start).TotalHours, 2);
        }

        public override string GetRemarks(double hours)
        {
            if (hours == Goal) return "";

            if (hours < Goal)
            {
                return $"Early Out. Hours left: {Goal - hours} hours";
            }
            
            return $"Overtime. Hours extended: {hours - Goal} hours";
        }
    }

    class Program
    {
        static Dictionary<string, LogEntry> EmployeeTimeinTimeOutRecord = new Dictionary<string, LogEntry>();
        static AttendanceProcessor processor = new AttendanceProcessor();

        static void Main(string[] args)
        {
            Console.WriteLine("--- Office Attendance Terminal ---");

            Console.Write("Enter Employee Number: ");
            string empID = Console.ReadLine();

            Console.Write("Enter Employee Name: ");
            string empName = Console.ReadLine();

            Console.Write("Office Location (Philippines, United States, India): ");
            string loc = Console.ReadLine();

            DateTime currentTime = GetRegionalTime(loc);

            Console.WriteLine($"\nYou clocked in at: {currentTime.ToString("MM/dd/yy hh:mm:ss tt")}");

            LogEntry entry = new LogEntry
            {
                ID = empID,
                Name = empName,
                Branch = loc,
                ClockIn = currentTime
            };

            Console.WriteLine("\n--- Processing Time-Out Simulation ---");
            Console.Write("How many hours did you work? ");
            string input = Console.ReadLine();
            
            if (double.TryParse(input, out double h))
            {
                entry.ClockOut = entry.ClockIn.AddHours(h);
                entry.HoursWorked = processor.GetTotalTime(entry.ClockIn, entry.ClockOut);
                entry.StatusNote = processor.GetRemarks(entry.HoursWorked);

                EmployeeTimeinTimeOutRecord[empID] = entry;

                Console.WriteLine("\n--- Employee Log Summary ---");
                Console.WriteLine($"Employee: {entry.Name} ({entry.ID})");
                Console.WriteLine($"Location: {entry.Branch}");
                Console.WriteLine($"Time-In: {entry.ClockIn.ToString("MM/dd/yy hh:mm:ss tt")}");
                Console.WriteLine($"Time-Out: {entry.ClockOut.ToString("MM/dd/yy hh:mm:ss tt")}");
                Console.WriteLine($"Total: {entry.HoursWorked} hrs");
                Console.WriteLine($"Note: {entry.StatusNote}");
            }
            else
            {
                Console.WriteLine("Invalid input. Please enter a number for hours.");
            }

            Console.WriteLine("\nPress any key to close...");
            Console.ReadKey();
        }

        static DateTime GetRegionalTime(string location)
        {
            string zoneId = location.ToLower() switch
            {
                "philippines" => "Singapore Standard Time",
                "united states" => "Eastern Standard Time",
                "india" => "India Standard Time",
                _ => "UTC"
            };

            try
            {
                var zone = TimeZoneInfo.FindSystemTimeZoneById(zoneId);
                return TimeZoneInfo.ConvertTimeFromUtc(DateTime.UtcNow, zone);
            }
            catch
            {
                return DateTime.Now;
            }
        }
    }
}