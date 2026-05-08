🏗️ DB-Delay-Tracker: Hamburg Hbf Real-Time Analysis

📊 Project Overview

#This project is a real-time data pipeline designed to monitor and analyze train reliability at Hamburg Hauptbahnhof. Using the Deutsche Bahn API, I captured 18,000+ records to visualize how delays evolve throughout the day and identify peak "unreliability" windows.

🛠️ The Tech Stack

##Python: Data collection via DB API and initial CSV logging.

##Excel / Pivot Tables: Data transformation, cleaning, and time-series visualization.

##Data Engineering: Custom logic to handle non-standard API time formats.

🔍 Engineering Challenges & Solutions
#1. The "Zombie Train" Problem (Data Latency)
Challenge: At 1:00 AM, the API often reports stale records from the previous afternoon (e.g., a train from 1:00 PM that never cleared the system).
#Solution: Implemented a "Stale-Record Filter" using a logical check between check_time and planned_arr. This prevented 12-hour-old data from skewing current reliability metrics.

#2. Non-Standard Time Math
Challenge: The API provides time in YYMMDDHHMM (Integer) format. Standard subtraction fails when a train is delayed across an hour boundary (e.g., 14:59 to 15:05 looks like a 46-unit jump instead of 6 minutes).
#Solution: Developed a robust Excel transformation to convert integers into total minutes from midnight:

=(INT(MOD(Actual,10000)/100)*60 + MOD(Actual,100)) - (INT(MOD(Planned,10000)/100)*60 + MOD(Planned,100))

#3. Midnight Rollover Logic
Challenge: Simple math breaks when a delay crosses 00:00.
#Solution: Integrated a date-aware calculation using MAX(0, ...) to ensure early arrivals are treated as "On Time" (0 mins) and late-night transitions are calculated accurately.

📈 Key Insights

#Morning Rush Hour: Average delays peaked at 23 minutes between 07:00 and 09:00.

#Line Reliability: Through interactive Slicers, I identified that the [Insert Line Name, e.g., RE8] showed the highest variance in performance.

#System Recovery: Data shows that the network typically recovers from morning delays by [Time], indicating effective mid-day buffer scheduling.

🚀 How to Run

#Run the main.py script to begin data collection.

#Open hamburg_delays.xlsx.

#Go to the Data tab and click Refresh All to update the Pivot Tables and Charts.
