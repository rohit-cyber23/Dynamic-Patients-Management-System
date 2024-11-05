# Dynamic-Patients-Management-System<br>

This C-based Dynamic Patient Management System enhances patient record management in healthcare. It supports unlimited entries with a user-friendly interface, capturing details like ID, name, age, gender, and contact information. Utilizing dynamic memory allocation, this project demonstrates effective software solutions for healthcare efficiency.<br>


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

    Patient newPatient;<br>
    newPatient.id = patientCount + 1;<br>

    printf("Enter patient name: ");<br>
    getchar();  // Consume newline character from previous input<br>
    fgets(newPatient.name, sizeof(newPatient.name), stdin);<br>
    newPatient.name[strcspn(newPatient.name, "\n")] = 0; // Remove newline<br>

    printf("Enter patient age: ");<br>
    scanf("%d", &newPatient.age);<br>

    printf("Enter patient gender: ");<br>
    getchar(); // Consume newline<br>
    fgets(newPatient.gender, sizeof(newPatient.gender), stdin);<br>
    newPatient.gender[strcspn(newPatient.gender, "\n")] = 0; // Remove newline<br>

    printf("Enter patient contact: ");<br>
    fgets(newPatient.contact, sizeof(newPatient.contact), stdin);<br>
    newPatient.contact[strcspn(newPatient.contact, "\n")] = 0; // Remove newline<br>

    patients[patientCount++] = newPatient;<br>
    printf("Patient added successfully!\n");<br>
}<br>

// Function to view all patients<br>
void viewPatients() {<br>
    if (patientCount == 0) {<br>
        printf("No patients to display.\n");<br>
        return;<br>
    }<br>

    printf("\nPatient List:\n");<br>
    printf("ID\tName\t\tAge\tGender\tContact\n");<br>
    for (int i = 0; i < patientCount; i++) {<br>
        printf("%d\t%s\t%d\t%s\t%s\n", patients[i].id, patients[i].name, patients[i].age, patients[i].gender, patients[i].contact);<br>
    }<br>
}<br>

// Function to update patient details<br>
void updatePatient() {<br>
    int id;<br>
    printf("Enter patient ID to update: ");<br>
    scanf("%d", &id);<br>

    if (id <= 0 || id > patientCount) {<br>
        printf("Invalid patient ID.\n");<br>
        return;<br>
    }<br>

    Patient *patient = &patients[id - 1];<br>

    printf("Updating details for %s:\n", patient->name);<br>
    printf("Enter new name (or press enter to keep current): ");<br>
    getchar();  // Consume newline<br>
    char newName[50];<br>
    fgets(newName, sizeof(newName), stdin);<br>
    if (strlen(newName) > 1) {<br>
        newName[strcspn(newName, "\n")] = 0; // Remove newline<br>
        strcpy(patient->name, newName);<br>
    }<br>

    printf("Enter new age (or enter -1 to keep current): ");<br>
    int newAge;<br>
    scanf("%d", &newAge);<br>
    if (newAge != -1) {<br>
        patient->age = newAge;<br>
    }<br>

    printf("Enter new gender (or press enter to keep current): ");<br>
    getchar();<br>
    char newGender[10];<br>
    fgets(newGender, sizeof(newGender), stdin);<br>
    if (strlen(newGender) > 1) {<br>
        newGender[strcspn(newGender, "\n")] = 0; // Remove newline<br>
        strcpy(patient->gender, newGender);<br>
    }<br>

    printf("Enter new contact (or press enter to keep current): ");<br>
    char newContact[15];<br>
    fgets(newContact, sizeof(newContact), stdin);<br>
    if (strlen(newContact) > 1) {<br>
        newContact[strcspn(newContact, "\n")] = 0; // Remove newline<br>
        strcpy(patient->contact, newContact);<br>
    }<br>

    printf("Patient updated successfully!\n");<br>
}<br>

// Function to free allocated memory<br>
void freeMemory() {<br>
    free(patients);<br>
}<br>

// Main function<br>
int main() {<br>
    int choice;<br>
    do {<br>
        printf("\n--- Patient Management System ---\n");<br>
        printf("1. Add Patient\n");<br>
        printf("2. View Patients\n");<br>
        printf("3. Update Patient\n");<br>
        printf("4. Exit\n");<br>
        printf("Enter your choice: ");<br>
        scanf("%d", &choice);<br>

        switch (choice) {<br>
            case 1:<br>
                addPatient();<br>
                break;<br>
            case 2:<br>
                viewPatients();<br>
                break;<br>
            case 3:<br>
                updatePatient();<br>
                break;<br>
            case 4:<br>
                printf("Exiting...\n");<br>
                break;<br>
            default:<br>
                printf("Invalid choice. Please try again.\n");<br>
        }<br>
    } while (choice != 4);<br>

    freeMemory(); // Free allocated memory before exiting<br>
    return 0;<br>
}<br>
