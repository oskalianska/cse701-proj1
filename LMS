#include <stdio.h>
#include <time.h>
#include <string.h>

// Minumum and maximum allowed values for ID
#define MIN_ID 0
#define MAX_ID 9999

// Minumum and maximum allowed values for AGE
#define MAX_AGE 70
#define MIN_AGE 18

// Minumum and maximum allowed values for SALARY
#define MAX_SALARY 300000
#define MIN_SALARY 15000

// Minumum and maximum allowed values for SPENDING
#define MAX_SPENDING 300000
#define MIN_SPENDING 0

// Name of the file on the disk, where database is written
#define FILE_NAME "records.dat"

/*
    Type that describes a record in the database.
    Fields:
        - id : integer number, uniquely identifying a record
        - age: age of a person
        - salary: salary of a person
        - spending: amount of money person spends on self-education
*/
typedef struct
{
    unsigned int id; 
    unsigned int age;
    unsigned int salary;
    unsigned int spending;
} data_record;

/*
    Helper function to get number of records in the datase.
    This function does not read all records, but only checks for the size of the file
    and calculates number of records by dividing file size by size of one record
*/
int number_of_records_in_file()
{
    FILE *fp = NULL;
    data_record record = {0};

    fp = fopen(FILE_NAME,"rb");
    if(fp == NULL)
    {
        // If file doesn't exist then consider it having 0 records
        return 0;
    }

    // Rewind cursor to the end of the file
    fseek(fp, 0L, SEEK_END);
    long rec_count = ftell(fp) / sizeof(record);
    fclose(fp);

    return rec_count;
}

/*
    This functon reads all records from the datase into an array.
    Length of array must be specified beforehand. 
    If file has less records, than length of an array passed as argument, not all elements of an array will have values
*/
void read_all_records(data_record *a)
{
    data_record record = {0};
    FILE *fp = NULL;

    fp = fopen(FILE_NAME,"rb");
    if(fp == NULL)
    {
        printf("File is not opened\n");
        return;
    }

    int i = 0;
    // read record by record from file into a variable record
    while (fread (&record, sizeof(record), 1, fp))
    {
        // copy values for an element of array with index i
        a[i].id = record.id;
        a[i].age = record.age;
        a[i].salary = record.salary;
        a[i].spending = record.spending;

        i++;
    }

    fclose(fp);
}

/*
    Function that validates entered id for a person.
    It checks against minumum and maximum allowed values. Additionally, checks if entered id was not used before
    by any of th existing records in the database.
*/
int valid_id(int id)
{
    if (id > MAX_ID || id < MIN_ID)
    {
        return 1;
    }

    int count = number_of_records_in_file();
    data_record res[count];
    read_all_records(res);
        
    for (int i = 0; i < count; i++)
    {
        if (res[i].id == (unsigned int)id)
        {
            return 2;
        }
    }

    return 0;
}

/*
    Function that validates age of a person.
    It checks against minumum and maximum allowed values.
*/
int valid_age(int age)
{
    if (age > MAX_AGE || age < MIN_AGE)
        return 0;

    return 1;    
}   

/*
    Function that validates salary of a person.
    It checks against minumum and maximum allowed values.
*/
int valid_salary(int salary)
{
    if (salary > MAX_SALARY || salary < MIN_SALARY)
        return 0;
    
    return 1;
}

/*
    Function that validates spending (amount of money, spent on self-education) of a person.
    It checks against minumum and maximum allowed values.
*/
int valid_spending(int spending)
{
    if (spending > MAX_SPENDING || spending < MIN_SPENDING)
        return 0;
        
    return 1;
}

/*
    Function that prompts end user to enter information for a record to be added to the databse.
    It validates each field's value as it is entered and asks to correct if value is not valid.
    If everything is entered correctly, record is saved in the database.
*/
void add_data()
{
    data_record new_record = {0};
    FILE *fp = NULL;
    int check = 0;
    
    // Open file for appending binary data
    fp = fopen(FILE_NAME,"ab+");

    // If for some reason file can not be opened, stop execution of a function
    if(fp == NULL)
    {
        printf("File is not opened\n");
        return;
    }

    printf("\nEnter data:\n\n");
    
    // Prompt user to enter ID
    do
    {
        int id = -1;
        printf("\nID = ");
        scanf("%u",&id);

        check = valid_id(id);
        switch(check)
        {
            case 1:
                printf("ID must be an integer between %u and %u\n", MIN_ID, MAX_ID);
                break;
            case 2:
                printf("ID is taken. Please, use a different one\n");
                break;
            case 0:
                new_record.id = (unsigned int)id;
                break;
        }
    }
    while (check != 0);

    // Prompt user to enter age
    do
    {
        printf("\nAge  = ");
        fflush(stdin);
        scanf("%u",&new_record.age);

        check = valid_age(new_record.age);
        if (!check)
        {
            printf("\nWrong age! Please enter again.");
        }
    }
    while (!check);

    // Prompt user to enter Salary
    do
    {
        printf("\nSalary  = ");
        fflush(stdin);
        scanf("%u",&new_record.salary);

        check = valid_salary(new_record.salary);
        if (!check)
        {
            printf("\nWrong value! Please enter again.");
        }
    }
    while (!check);

    // Prompt user to enter spending on self-education
    do
    {
        printf("\nSpending on self-education  = ");
        fflush(stdin);
        scanf("%u",&new_record.spending);

        check = valid_spending(new_record.spending);
        if (!check)
        {
            printf("\nWrong value! Please enter again.");
        }
    }
    while(!check);
    
    // Save entered data in the datavbase file
    fwrite(&new_record,sizeof(new_record), 1, fp);

    fclose(fp);
} 

/*
    Function outputs all records from the database file on the screen.
    Each record is shown from the new line, prepended by its ID.
*/
void view_data()
{
    data_record current_record = {0};
    FILE *fp = NULL;
    unsigned int count_data = 0;

    fp = fopen(FILE_NAME,"rb");

    // if file can not be opened, then exit from the method
    if(fp == NULL)
    {
        printf("File can not be opened\n");
        return;
    }

    while (fread (&current_record, sizeof(current_record), 1, fp))
    {
        printf("\nRecord number %d:\n",count_data+1);
        printf("\tID = %u",current_record.id);
        printf("\tAge = %u",current_record.age);
        printf("\tSalary = %u",current_record.salary);
        printf("\tSpendings on education resourses = %u\n",current_record.spending);
        count_data++;
    }
    fclose(fp);

    // if file was opened, but all records from the file were previously deleted, then show a message
    if(count_data == 0)
    {
        printf("\nNo Records Found");
    }

    printf("\nPress any key to go to main menu...");
    getchar();
}

/*
    Calculate a sum of all elements in an array
*/
long accumulate(int *a, int size)
{
    long res = 0;
    for (int i=0; i<size; i++)
    {
        res += a[i];
    }
    return res;
}

/*
    This method performs linear regression.
    It calculates slope and intercept for the two sets of data, represented as two arrays x and y of the same size.
    Length of the arrays is passed in parameter n.
*/
void coef(int *x, int *y, int n)
{
    //EY = b0+b1*x
    long sum_Y = accumulate(y, n);
    long sum_X = accumulate(x, n);
    long sum_XY = sum_Y*sum_X;

    float b1 = (n*sum_XY-sum_X*sum_Y)/(n*sum_X*sum_X-sum_X*sum_X);
    float b0 = (sum_Y - b1*sum_X)/n;

    printf("\nCoefficient b0 is %.2f and b1 is %.2f", b0, b1);
    printf("\nEquation of best fit is: y = %.2f + %.2f * x", b0, b1);
}

/*
    This method displayes results of performing a linear regression.
    It loads all data from the database and models the relationship between a dependent variable (salary) 
    and independent variable (spending). After performing calculations, best fit equation is displayed.
*/
void analyze_data()
{
    int count = number_of_records_in_file();
    data_record res[count];
    read_all_records(res);

    int x [count];
    int y [count];

    for (int i=0; i<count; i++)
    {
        x[i] = res[i].spending;
        y[i] = res[i].salary;
    }

    coef(x, y, count);
    printf("\n\nPress any key to go to main menu...");
    fflush(stdin);
    getchar();
}

/*
    Method that implements deletion of a record from the database.
    It prompts user for an ID of a record, which needs to be deleted.
    If record is found in the database, then all information about that record is removed.
*/
void delete_data()
{
    int found = 0;
    unsigned int delete = 0;
    data_record record_to_delete = {0};
    FILE *fp = NULL;
    FILE *bak = NULL;

    // open database file in read mode for finding a record by ID
    fp = fopen(FILE_NAME,"rb");
    if(fp == NULL)
    {
        printf("File is not opened\n");
        return;
    }

    // create a temporary file that will contnain updated information, without the record to be deleted
    // if unable to create a backup file, then execution of the program is stopped
    bak = fopen("tmp.dat","wb");
    if(bak == NULL)
    {
        fclose(fp);
        printf("File is not opened\n");
        return;
    }

    // prompt user for an ID of a record to be removed from database 
    printf("\nEnter ID for delete:");
    scanf("%d",&delete);
    while (fread (&record_to_delete, sizeof(record_to_delete), 1, fp))
    {
        if(record_to_delete.id != delete)
        {
            // append record to the temporary file if id does not match
            fwrite(&record_to_delete, sizeof(record_to_delete), 1, bak);
        }
        else
        {
            // set flag that record to be deleted was found
            found = 1;
        }
    }

    fclose(fp);
    fclose(bak);

    remove(FILE_NAME);
    rename("tmp.dat",FILE_NAME);
}

/*
    This method displays menu of possible actions to the end-user.
    Each command, supported by the program, can be triggered by selecting an appropriate option.
    Available options are associated by numbers from 0 to 4.
    User is promted to enter a number for the command and guided through the process with additioanal prompts.
*/
void menu()
{
    int choice = 0;
    do
    {
        // print main menu
        printf("\n1.Add data");
        printf("\n2.View data");
        printf("\n3.Analyze data");
        printf("\n4.Delete data");
        printf("\n0.Exit");
        printf("\n\nPlease enter your choice: ");

        // read user's choice
        scanf("%d",&choice);

        // perform appropriate command, based on selected option
        switch(choice)
        {
            case 1:
                add_data(); break;
            case 2:
                view_data(); break;
            case 3:
                analyze_data(); break;
            case 4:
                delete_data(); break;
            case 0:
                printf("\nBye!");
            default:
                printf("\nWrong choice. Please try again");
        }
    }
    while(choice!=0); // if choice is 0 then stop the while loop and return control to the calling method
}

/*
    This method is an entry point into the program.
    It displays menu (list of available commands) and when control from menu is returned here (user selected Exit command), 
    then program exists with a success code
*/
int main()
{
    printf("Welcome to data management system\n");
    menu();
    return 0;
}
