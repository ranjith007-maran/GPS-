# GPS toll based system simulation using python
my first repository on GitHub
The GPS Toll-Based System Simulation project models a toll booth system using GPS data to calculate toll charges. This simulation is implemented using the SimPy library and is organized into multiple modules for clarity and maintainability.
I have created three modules, 1.gps.py 2.gps_module.py 3.simulation.py

1.gps_module code

import random

TOLL_RATE_PER_KM = 3  

def generate_gps_data():
    return random.uniform(1, 100)

def calculate_toll(distance):
    return distance * TOLL_RATE_PER_KM
    
USECASE:
The gps_module.py contains functions related to generating GPS data and calculating toll charges based on the distance traveled. This module abstracts the GPS and toll calculation logic, making the code modular and easier to maintain.

Functions in gps_module.py

generate_gps_data(): This function simulates the generation of GPS data, specifically the distance traveled by a vehicle.
calculate_toll(distance): This function calculates the toll based on the distance traveled using a predefined toll rate.

2.Simulation.py

import simpy
import random
from gps_module import generate_gps_data, calculate_toll

TOLL_BOOTHS = 2  
ARRIVAL_INTERVAL = 5  
PROCESSING_TIME = [2, 5] 
SIMULATION_TIME = 100 

    def vehicle(environment, name, toll_booth):

    arrival_time = environment.now
    print(f'{name} arrives at the toll booth at {arrival_time:.2f}')
    # Simulate GPS data for the vehicle
    
    distance_traveled = generate_gps_data()
    toll = calculate_toll(distance_traveled)

    print(f'{name} has traveled {distance_traveled:.2f} km and needs to pay a toll of ₹{toll:.2f}')

    with toll_booth.request() as request:
        yield request
        processing_time = random.uniform(*PROCESSING_TIME)
        print(f'{name} starts being processed at {environment.now:.2f}')
        yield environment.timeout(processing_time)
        print(f'{name} leaves the toll booth at {environment.now:.2f} (Processed in {processing_time:.2f} minutes)')


    def setup(environment, toll_booths):
    """Function to setup the toll booths and generate vehicles."""
    toll_booth = simpy.Resource(environment, toll_booths)

    # Generate vehicles at random intervals
    i = 0
    while True:
        yield environment.timeout(random.expovariate(1 / ARRIVAL_INTERVAL))
        i += 1
        environment.process(vehicle(environment, f'Vehicle {i}', toll_booth))


# Create the simulation environment
simulation_env = simpy.Environment()
simulation_env.process(setup(simulation_env, TOLL_BOOTHS))

# Run the simulation
simulation_env.run(until=SIMULATION_TIME)

USECASE:

the simulation.py module contains the core logic for the GPS toll-based system simulation. It defines the processes for vehicles arriving at the toll booth, getting processed, and departing. It utilizes the simpy library to model the simulation environment and manages the flow of vehicles through the toll booths.

Key Components in simulation.py
Vehicle Process: Simulates the lifecycle of a vehicle at the toll booth.
Setup Function: Initializes the simulation environment and starts the vehicle generation process.
Run Simulation Function: Runs the simulation for a specified amount of time

FUNCTIONS:

Vehicle Process:

vehicle(environment, name, toll_booth): This function simulates a vehicle arriving at the toll booth, being processed, and then leaving.
Upon arrival, the vehicle generates GPS data to determine the distance traveled (generate_gps_data()) and calculates the toll (calculate_toll(distance)).
The vehicle requests access to a toll booth (toll_booth.request()). Once the request is granted, the vehicle undergoes a processing delay (random.uniform(*PROCESSING_TIME)) before departing.
Setup Function:

setup(environment, toll_booths): This function initializes the simulation environment with the specified number of toll booths.
It generates vehicles at random intervals (random.expovariate(1 / ARRIVAL_INTERVAL)) and starts the vehicle process.
Run Simulation Function:

run_simulation(): This function sets up the simulation environment and starts the simulation process.
The simulation runs for a specified duration (SIMULATION_TIME), during which vehicles arrive, get processed, and leave the toll booth.

OUTPUT:
E:\python.exe C:/Users/RANJITH/PycharmProjects/pythonProject/simulation.py 
Vehicle 1 arrives at the toll booth at 1.82
Vehicle 1 has traveled 19.60 km and needs to pay a toll of ₹58.79
Vehicle 1 starts being processed at 1.82
Vehicle 2 arrives at the toll booth at 2.96
Vehicle 2 has traveled 22.56 km and needs to pay a toll of ₹67.68
Vehicle 2 starts being processed at 2.96
Vehicle 1 leaves the toll booth at 4.32 (Processed in 2.50 minutes)
Vehicle 2 leaves the toll booth at 6.40 (Processed in 3.44 minutes)
Vehicle 3 arrives at the toll booth at 9.58
Vehicle 3 has traveled 17.24 km and needs to pay a toll of ₹51.72
Vehicle 3 starts being processed at 9.58
Vehicle 4 arrives at the toll booth at 11.04
Vehicle 4 has traveled 78.90 km and needs to pay a toll of ₹236.70
Vehicle 4 starts being processed at 11.04
Vehicle 5 arrives at the toll booth at 12.23
Vehicle 5 has traveled 6.07 km and needs to pay a toll of ₹18.22
Vehicle 4 leaves the toll booth at 13.80 (Processed in 2.76 minutes)
Vehicle 5 starts being processed at 13.80
Vehicle 6 arrives at the toll booth at 14.11
Vehicle 6 has traveled 99.82 km and needs to pay a toll of ₹299.46
Vehicle 3 leaves the toll booth at 14.52 (Processed in 4.94 minutes)
Vehicle 6 starts being processed at 14.52
Vehicle 5 leaves the toll booth at 16.68 (Processed in 2.88 minutes)
Vehicle 6 leaves the toll booth at 18.13 (Processed in 3.62 minutes)
Vehicle 7 arrives at the toll booth at 18.55
Vehicle 7 has traveled 98.80 km and needs to pay a toll of ₹296.40
Vehicle 7 starts being processed at 18.55
Vehicle 8 arrives at the toll booth at 20.15
Vehicle 8 has traveled 38.60 km and needs to pay a toll of ₹115.81
Vehicle 8 starts being processed at 20.15
Vehicle 7 leaves the toll booth at 23.33 (Processed in 4.79 minutes)
Vehicle 8 leaves the toll booth at 24.25 (Processed in 4.10 minutes)
Vehicle 9 arrives at the toll booth at 24.64
Vehicle 9 has traveled 81.61 km and needs to pay a toll of ₹244.84
Vehicle 9 starts being processed at 24.64
Vehicle 9 leaves the toll booth at 27.61 (Processed in 2.97 minutes)
Vehicle 10 arrives at the toll booth at 29.78
Vehicle 10 has traveled 40.99 km and needs to pay a toll of ₹122.98
Vehicle 10 starts being processed at 29.78
Vehicle 11 arrives at the toll booth at 31.57
Vehicle 11 has traveled 50.13 km and needs to pay a toll of ₹150.39
Vehicle 11 starts being processed at 31.57
Vehicle 12 arrives at the toll booth at 33.63
Vehicle 12 has traveled 58.37 km and needs to pay a toll of ₹175.12
Vehicle 10 leaves the toll booth at 34.50 (Processed in 4.72 minutes)
Vehicle 12 starts being processed at 34.50
Vehicle 11 leaves the toll booth at 35.74 (Processed in 4.17 minutes)
Vehicle 12 leaves the toll booth at 39.32 (Processed in 4.82 minutes)
Vehicle 13 arrives at the toll booth at 39.75
Vehicle 13 has traveled 46.53 km and needs to pay a toll of ₹139.58
Vehicle 13 starts being processed at 39.75
Vehicle 13 leaves the toll booth at 41.82 (Processed in 2.08 minutes)
Vehicle 14 arrives at the toll booth at 46.28
Vehicle 14 has traveled 63.43 km and needs to pay a toll of ₹190.29
Vehicle 14 starts being processed at 46.28
Vehicle 14 leaves the toll booth at 49.30 (Processed in 3.02 minutes)
Vehicle 15 arrives at the toll booth at 53.37
Vehicle 15 has traveled 27.94 km and needs to pay a toll of ₹83.82
Vehicle 15 starts being processed at 53.37
Vehicle 16 arrives at the toll booth at 54.42
Vehicle 16 has traveled 62.63 km and needs to pay a toll of ₹187.90
Vehicle 16 starts being processed at 54.42
Vehicle 15 leaves the toll booth at 57.63 (Processed in 4.26 minutes)
Vehicle 16 leaves the toll booth at 57.86 (Processed in 3.43 minutes)
Vehicle 17 arrives at the toll booth at 59.37
Vehicle 17 has traveled 47.35 km and needs to pay a toll of ₹142.05
Vehicle 17 starts being processed at 59.37
Vehicle 18 arrives at the toll booth at 59.69
Vehicle 18 has traveled 3.32 km and needs to pay a toll of ₹9.96
Vehicle 18 starts being processed at 59.69
Vehicle 19 arrives at the toll booth at 61.28
Vehicle 19 has traveled 80.31 km and needs to pay a toll of ₹240.93
Vehicle 18 leaves the toll booth at 63.53 (Processed in 3.84 minutes)
Vehicle 19 starts being processed at 63.53
Vehicle 17 leaves the toll booth at 63.96 (Processed in 4.60 minutes)
Vehicle 19 leaves the toll booth at 67.33 (Processed in 3.81 minutes)
Vehicle 20 arrives at the toll booth at 70.37
Vehicle 20 has traveled 28.65 km and needs to pay a toll of ₹85.94
Vehicle 20 starts being processed at 70.37
Vehicle 21 arrives at the toll booth at 72.08
Vehicle 21 has traveled 10.07 km and needs to pay a toll of ₹30.20
Vehicle 21 starts being processed at 72.08
Vehicle 22 arrives at the toll booth at 72.99
Vehicle 22 has traveled 56.04 km and needs to pay a toll of ₹168.13
Vehicle 20 leaves the toll booth at 73.15 (Processed in 2.78 minutes)
Vehicle 22 starts being processed at 73.15
Vehicle 23 arrives at the toll booth at 74.20
Vehicle 23 has traveled 57.45 km and needs to pay a toll of ₹172.35
Vehicle 21 leaves the toll booth at 76.19 (Processed in 4.11 minutes)
Vehicle 23 starts being processed at 76.19
Vehicle 22 leaves the toll booth at 76.61 (Processed in 3.46 minutes)
Vehicle 23 leaves the toll booth at 79.69 (Processed in 3.50 minutes)
Vehicle 24 arrives at the toll booth at 80.65
Vehicle 24 has traveled 6.43 km and needs to pay a toll of ₹19.29
Vehicle 24 starts being processed at 80.65
Vehicle 25 arrives at the toll booth at 83.04
Vehicle 25 has traveled 67.92 km and needs to pay a toll of ₹203.76
Vehicle 25 starts being processed at 83.04
Vehicle 26 arrives at the toll booth at 83.19
Vehicle 26 has traveled 43.07 km and needs to pay a toll of ₹129.22
Vehicle 24 leaves the toll booth at 84.42 (Processed in 3.77 minutes)
Vehicle 26 starts being processed at 84.42
Vehicle 27 arrives at the toll booth at 84.84
Vehicle 27 has traveled 99.88 km and needs to pay a toll of ₹299.63
Vehicle 28 arrives at the toll booth at 86.06
Vehicle 28 has traveled 5.31 km and needs to pay a toll of ₹15.94
Vehicle 25 leaves the toll booth at 86.52 (Processed in 3.48 minutes)
Vehicle 27 starts being processed at 86.52
Vehicle 26 leaves the toll booth at 89.27 (Processed in 4.85 minutes)
Vehicle 28 starts being processed at 89.27
Vehicle 27 leaves the toll booth at 89.32 (Processed in 2.80 minutes)
Vehicle 28 leaves the toll booth at 91.57 (Processed in 2.30 minutes)
Vehicle 29 arrives at the toll booth at 94.92
Vehicle 29 has traveled 94.41 km and needs to pay a toll of ₹283.22
Vehicle 29 starts being processed at 94.92
Vehicle 30 arrives at the toll booth at 95.45
Vehicle 30 has traveled 11.43 km and needs to pay a toll of ₹34.28
Vehicle 30 starts being processed at 95.45
Vehicle 30 leaves the toll booth at 98.44 (Processed in 2.99 minutes)
Vehicle 31 arrives at the toll booth at 98.74
Vehicle 31 has traveled 6.45 km and needs to pay a toll of ₹19.34
Vehicle 31 starts being processed at 98.74
Vehicle 29 leaves the toll booth at 99.11 (Processed in 4.19 minutes)
Vehicle 32 arrives at the toll booth at 99.67
Vehicle 32 has traveled 24.72 km and needs to pay a toll of ₹74.16
Vehicle 32 starts being processed at 99.67

Process finished with exit code 0

CONCLUSION:
Using PyCharm for the GPS Toll-Based System Simulation project provides several benefits, including enhanced code navigation, powerful debugging tools, and integrated version control. This setup allows you to focus on writing and improving your simulation code with the support of a robust development environment. Once completed, you can share your project on GitHub, making it accessible to others for collaboration and further development.
    

