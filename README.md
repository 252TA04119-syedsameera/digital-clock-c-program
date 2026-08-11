#include <stdio.h>

struct Clock
{
    int hours;
    int minutes;
    int seconds;
};

void displayTime(struct Clock c)
{
    printf("\nCurrent Time: %02d:%02d:%02d\n",
           c.hours, c.minutes, c.seconds);
}

int isValidTime(struct Clock c)
{
    if (c.hours >= 0 && c.hours < 24 &&
        c.minutes >= 0 && c.minutes < 60 &&
        c.seconds >= 0 && c.seconds < 60)
    {
        return 1;
    }
    return 0;
}

void addSeconds(struct Clock *c, int seconds)
{
    c->seconds += seconds;

    while (c->seconds >= 60)
    {
        c->seconds -= 60;
        c->minutes++;
    }

    while (c->minutes >= 60)
    {
        c->minutes -= 60;
        c->hours++;
    }

    while (c->hours >= 24)
    {
        c->hours -= 24;
    }
}

void convertTo12Hour(struct Clock c)
{
    int hour;
    char period[3];

    if (c.hours == 0)
    {
        hour = 12;
        period[0] = 'A';
        period[1] = 'M';
    }
    else if (c.hours < 12)
    {
        hour = c.hours;
        period[0] = 'A';
        period[1] = 'M';
    }
    else
    {
        hour = (c.hours == 12) ? 12 : c.hours - 12;
        period[0] = 'P';
        period[1] = 'M';
    }

    period[2] = '\0';

    printf("12-Hour Format: %02d:%02d:%02d %s\n",
           hour, c.minutes, c.seconds, period);
}

int main()
{
    struct Clock c;
    int choice;
    int extraSeconds;

    printf("====================================\n");
    printf("          DIGITAL CLOCK\n");
    printf("====================================\n");

    printf("Enter hours (0-23): ");
    scanf("%d", &c.hours);

    printf("Enter minutes (0-59): ");
    scanf("%d", &c.minutes);

    printf("Enter seconds (0-59): ");
    scanf("%d", &c.seconds);

    if (!isValidTime(c))
    {
        printf("\nInvalid time entered!\n");
        return 0;
    }

    do
    {
        printf("\n========== CLOCK MENU ==========\n");
        printf("1. Display Time\n");
        printf("2. Convert to 12-Hour Format\n");
        printf("3. Add Seconds\n");
        printf("4. Check Time Validity\n");
        printf("5. Exit\n");
        printf("================================\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                displayTime(c);
                break;

            case 2:
                convertTo12Hour(c);
                break;

            case 3:
                printf("Enter seconds to add: ");
                scanf("%d", &extraSeconds);

                if (extraSeconds >= 0)
                {
                    addSeconds(&c, extraSeconds);
                    printf("Time after adding seconds:\n");
                    displayTime(c);
                }
                else
                {
                    printf("Please enter a positive value.\n");
                }
                break;

            case 4:
                if (isValidTime(c))
                    printf("The time is valid.\n");
                else
                    printf("The time is invalid.\n");
                break;

            case 5:
                printf("\nExiting the clock program...\n");
                break;

            default:
                printf("\nInvalid choice! Please try again.\n");
        }

    } while (choice != 5);

    printf("\nThank you for using the Digital Clock Program!\n");

    return 0;
}
