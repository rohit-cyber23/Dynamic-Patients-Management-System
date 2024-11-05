# Dynamic-Patients-Management-System<br>

This C-based Dynamic Patient Management System enhances patient record management in healthcare. It supports unlimited entries with a user-friendly interface, capturing details like ID, name, age, gender, and contact information. Utilizing dynamic memory allocation, this project demonstrates effective software solutions for healthcare efficiency.<br>
<br>

#include <stdio.h><br>
#include <stdlib.h><br>
#include <string.h><br>

// Define the maximum number of patients (dynamic)<br>
typedef struct {<br>
    int id;                 // Patient ID<br>
    char name[50];         // Patient Name<br>
    int age;               // Patient Age<br>
    char gender[10];       // Patient Gender<br>
    char contact[15];      // Patient Contact Number<br>
} Patient;<br>

Patient *patients = NULL; // Pointer to hold the patient records<br>
int patientCount = 0;     // Current number of patients<br>
int patientCapacity = 0;  // Current capacity of the array<br>

// Function to add a new patient<br>
void addPatient() {<br>
    // Increase capacity if needed<br>
    if (patientCount >= patientCapacity) {<br>
        patientCapacity = patientCapacity == 0 ? 1 : patientCapacity * 2; // Double the capacity<br>
        patients = realloc(patients, patientCapacity * sizeof(Patient)); // Reallocate memory<br>
        if (patients == NULL) {<br>
            printf("Memory allocation failed!\n");<br>
            exit(1);
        }<br>
    }<br>
<br>
    Patient newPatient;<br>
    newPatient.id = patientCount + 1;<br>
<br>
    printf("Enter patient name: ");<br>
    getchar();  // Consume newline character from previous input<br>
    fgets(newPatient.name, sizeof(newPatient.name), stdin);<br>
    newPatient.name[strcspn(newPatient.name, "\n")] = 0; // Remove newline<br>
<br>
    printf("Enter patient age: ");<br>
    scanf("%d", &newPatient.age);<br>
<br>
    printf("Enter patient gender: ");
    getchar(); // Consume newline
    fgets(newPatient.gender, sizeof(newPatient.gender), stdin);
    newPatient.gender[strcspn(newPatient.gender, "\n")] = 0; // Remove newline

    printf("Enter patient contact: ");
    fgets(newPatient.contact, sizeof(newPatient.contact), stdin);
    newPatient.contact[strcspn(newPatient.contact, "\n")] = 0; // Remove newline

    patients[patientCount++] = newPatient;
    printf("Patient added successfully!\n");
}

// Function to view all patients
void viewPatients() {
    if (patientCount == 0) {
        printf("No patients to display.\n");
        return;
    }

    printf("\nPatient List:\n");
    printf("ID\tName\t\tAge\tGender\tContact\n");
    for (int i = 0; i < patientCount; i++) {
        printf("%d\t%s\t%d\t%s\t%s\n", patients[i].id, patients[i].name, patients[i].age, patients[i].gender, patients[i].contact);
    }
}

// Function to update patient details
void updatePatient() {
    int id;
    printf("Enter patient ID to update: ");
    scanf("%d", &id);

    if (id <= 0 || id > patientCount) {
        printf("Invalid patient ID.\n");
        return;
    }

    Patient *patient = &patients[id - 1];

    printf("Updating details for %s:\n", patient->name);
    printf("Enter new name (or press enter to keep current): ");
    getchar();  // Consume newline
    char newName[50];
    fgets(newName, sizeof(newName), stdin);
    if (strlen(newName) > 1) {
        newName[strcspn(newName, "\n")] = 0; // Remove newline
        strcpy(patient->name, newName);
    }

    printf("Enter new age (or enter -1 to keep current): ");
    int newAge;
    scanf("%d", &newAge);
    if (newAge != -1) {
        patient->age = newAge;
    }

    printf("Enter new gender (or press enter to keep current): ");
    getchar();
    char newGender[10];
    fgets(newGender, sizeof(newGender), stdin);
    if (strlen(newGender) > 1) {
        newGender[strcspn(newGender, "\n")] = 0; // Remove newline
        strcpy(patient->gender, newGender);
    }

    printf("Enter new contact (or press enter to keep current): ");
    char newContact[15];
    fgets(newContact, sizeof(newContact), stdin);
    if (strlen(newContact) > 1) {
        newContact[strcspn(newContact, "\n")] = 0; // Remove newline
        strcpy(patient->contact, newContact);
    }

    printf("Patient updated successfully!\n");
}

// Function to free allocated memory
void freeMemory() {
    free(patients);
}

// Main function
int main() {
    int choice;
    do {
        printf("\n--- Patient Management System ---\n");
        printf("1. Add Patient\n");
        printf("2. View Patients\n");
        printf("3. Update Patient\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addPatient();
                break;
            case 2:
                viewPatients();
                break;
            case 3:
                updatePatient();
                break;
            case 4:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 4);

    freeMemory(); // Free allocated memory before exiting
    return 0;
}
