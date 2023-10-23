# PeriodsLib
Creat by : Денис Казанцев & Артем Николаев

-Lib

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    
    namespace PeriodsLib.dll
    {
        public class Calculations
        {
             public static string[] qwemafie(TimeSpan[] startTimes, int[] durations, TimeSpan beginWorkingTime, TimeSpan endWorkingTime, int consultationTime)
            {
                if (startTimes.Length != durations.Length)
                {
                    throw new Exception("Длинна startTimes и durations не совпадает");
                }

            List<string> availablePeriods = new List<string>();
            int n = startTimes.Length;

            int beginMinutes = (int)beginWorkingTime.TotalMinutes;
            int endMinutes = (int)endWorkingTime.TotalMinutes;
            int currentTime = beginMinutes;

            for (int i = 0; i < n; i++)
            {
                int startTimeMinutes = (int)startTimes[i].TotalMinutes;
                int endTimeMinutes = startTimeMinutes + durations[i];

                if (startTimeMinutes - currentTime >= consultationTime)
                {
                    while (currentTime + consultationTime <= startTimeMinutes)
                    {
                        availablePeriods.Add(TimeSpan.FromMinutes(currentTime).ToString(@"hh\:mm"));
                        currentTime += consultationTime;
                    }
                }
                currentTime = endTimeMinutes;
            }

            if (endMinutes - currentTime >= consultationTime)
            {
                while (currentTime + consultationTime <= endMinutes)
                {
                    availablePeriods.Add(TimeSpan.FromMinutes(currentTime).ToString(@"hh\:mm"));
                    currentTime += consultationTime;
                }
            }
            string[] array = availablePeriods.ToArray();

            return (array);
        }
    }
    }


-Test

    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System;
    using PeriodsLib.dll;
    
    namespace PeriodsLibTest
    {
        [TestClass]
        public class TestMethod_PeriodsLibTest
        {
            [TestMethod]
            public void Check_ArrayComparison_Correct()
            {
                string[] qwe = new string[] { "08:00","08:30","09:00","09:30","11:30","12:00","12:30","13:00","13:30","14:00","14:30","15:40","16:10","17:30"};
                //Arrange
                TimeSpan[] startTimes = new TimeSpan[]
                {
                    TimeSpan.Parse("10:00"),
                    TimeSpan.Parse("11:00"),
                    TimeSpan.Parse("15:00"),
                    TimeSpan.Parse("15:30"),
                    TimeSpan.Parse("16:50")
                };
                int[] durations = new int[] { 60, 30, 10, 10, 40 };
                TimeSpan beginWorkingTime = TimeSpan.Parse("08:00");
                TimeSpan endWorkingTime = TimeSpan.Parse("18:00");
                int consultationTime = 30;
                //Act
                string[] exeption = Calculations.qwemafie(startTimes, durations, beginWorkingTime, endWorkingTime, consultationTime);
                //Assert
                CollectionAssert.AreEqual(qwe, exeption);
            }
            [TestMethod]
            public void Check_ArrayComparison_Correc2()
            {
                string[] qwe = new string[] { "11:20", "11:40", "12:00", "12:20", "12:40", "13:00", "13:20", "13:40", "14:10", "14:30","14:50","15:10","15:30","15:50","16:10","16:30","16:50","17:10","18:10","18:30" };
                //Arrange
                TimeSpan[] startTimes = new TimeSpan[]
                {
                    TimeSpan.Parse("10:00"),
                    TimeSpan.Parse("11:00"),
                    TimeSpan.Parse("14:00"),
                    TimeSpan.Parse("17:30")
                };
    
                int[] durations = new int[] { 60, 20, 10, 40 };
                TimeSpan beginWorkingTime = TimeSpan.Parse("10:00");
                TimeSpan endWorkingTime = TimeSpan.Parse("19:00");
                int consultationTime = 20;
                //Act
                string[] exeption = Calculations.qwemafie(startTimes, durations, beginWorkingTime, endWorkingTime, consultationTime);
                //Assert
                CollectionAssert.AreEqual(qwe, exeption);
            }
            [TestMethod]
            public void Check_ArrayComparison_LengthDoesNotMatch()
            {
                string[] qwe = new string[] { "08:00", "08:30", "09:00", "09:30", "11:30", "12:00", "12:30", "13:00", "13:30", "14:00", "14:30", "15:40", "16:10", "17:30" };
                //Arrange
                TimeSpan[] startTimes = new TimeSpan[]
                {
                    TimeSpan.Parse("10:00"),
                    TimeSpan.Parse("11:00"),
                    TimeSpan.Parse("15:00"),
                    TimeSpan.Parse("15:30")
                };
                int[] durations = new int[] { 60, 30, 10, 10, 40 };
                TimeSpan beginWorkingTime = TimeSpan.Parse("08:00");
                TimeSpan endWorkingTime = TimeSpan.Parse("18:00");
                int consultationTime = 30;
                //Act
                Action exeption = () =>Calculations.qwemafie(startTimes, durations, beginWorkingTime, endWorkingTime, consultationTime);
                //Assert
                Assert.ThrowsException<Exception>(exeption);
            }
        }
      }
