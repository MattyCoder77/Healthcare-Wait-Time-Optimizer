import re

# Function to get a valid patient name
def get_valid_name():
    while True:
        name = input("Enter your name: ").strip()
        if name:
            return name  # Return valid name
        print("Name cannot be empty.")  # Prompt user again if empty

# Function to get a valid symptom from the user
def get_valid_symptom():
    valid_symptoms = {"fever", "cough", "heart", "other"}
    while True:
        symptom = input("Enter your most severe symptom (fever, cough, heart, other): ").strip().lower()
        if symptom in valid_symptoms:
            return symptom  # Return valid symptom
        print("Invalid symptom. Valid options: fever, cough, heart, other.")  # Prompt again if invalid

# Function to get a valid urgency level (1, 2, or 3)
def get_valid_urgency_level():
    while True:
        try:
            urgency_level = int(input("Enter your urgency level (1 = mild, 2 = moderate, 3 = severe): ").strip())
            if urgency_level in {1, 2, 3}:
                return urgency_level  # Return valid urgency level
            else:
                print("Invalid urgency level. Enter 1, 2, or 3.")  # Prompt again if out of range
        except ValueError:
            print("Invalid input. Please enter a number (1, 2, or 3).")  # Handle non-integer input

# Function to calculate wait time for a hospital
def calculate_wait_time(patients_ahead, base_time_per_patient, urgency_level):
    return round((patients_ahead * base_time_per_patient) / urgency_level, 1)  # Apply given formula

# Main function
def main():
    # Step 1: Collect patient input
    name = get_valid_name()
    symptom = get_valid_symptom()
    urgency_level = get_valid_urgency_level()

    # Step 2: Define hospital data
    hospital_data = {
        "Victoria Hospital": {"patients_ahead": 15, "base_time_per_patient": 10},
        "St. Joseph’s Hospital": {"patients_ahead": 10, "base_time_per_patient": 12},
        "London Health Sciences Centre": {"patients_ahead": 20, "base_time_per_patient": 8},
    }

    # Step 3: Calculate wait times for all hospitals
    wait_times = {}
    for hospital, data in hospital_data.items():
        wait_times[hospital] = calculate_wait_time(data["patients_ahead"], data["base_time_per_patient"], urgency_level)

    # Step 4: Determine the specialty hospital based on symptoms
    specialty_hospital = {
        "fever": "Victoria Hospital",
        "cough": "St. Joseph’s Hospital",
        "heart": "London Health Sciences Centre",
        "other": None  # No specialty hospital for "other"
    }
    recommended_hospital = specialty_hospital[symptom]

    # Step 5: Identify the hospital with the shortest wait time
    fastest_hospital = min(wait_times, key=wait_times.get)
    fastest_time = wait_times[fastest_hospital]

    # Step 6: Display formatted results
    print("\n--- Results ---")
    print(f"Patient Name: {name}")
    print(f"Symptoms: {symptom.capitalize()}")
    print(f"Urgency Level: {urgency_level}")
    
    # Print the wait times for each hospital
    for hospital, time in wait_times.items():
        print(f"{hospital} Wait Time: {time} minutes")
    
    # If a specialty hospital exists for the symptom, compare wait times
    if recommended_hospital:
        specialty_time = wait_times[recommended_hospital]
        difference = round(abs(specialty_time - fastest_time), 1)
        print(f"Specialty hospital wait time: {specialty_time} minutes. Fastest hospital: {fastest_hospital} with {fastest_time} minutes. Difference: {difference} minutes.")
    else:
        print(f"Fastest hospital: {fastest_hospital} with {fastest_time} minutes. No specialty hospital for your symptom.")

# Run the program if executed directly
if __name__ == "__main__":
    main()
