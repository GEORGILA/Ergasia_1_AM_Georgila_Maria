# InfoSys Project 1

## Ερώτημα 1

### Τρόπος υλοποίησης
Η συνάρτηση find_one μας επιστρέφει αποτέλεσμα μόνο αν υπάρχει το username που αναζητούμε. Επομένως ελέγχοντας αν έχει επιστρέψει None, ελέγχουμε το αν υπάρχει ήδη το username στο Users.

Στην περίπτωση που δεν υπάρχει, με την χρήση της insert_one, εισάγουμε τον νέο χρήστη. 

Επιστρέφουμε το status 200 σε περίπτωση επιτυχίας και το status 400 σε περίπτωση σφάλματος.

### Έλεγχος ορθής λειτουργείας
POST request στο http://localhost:5000/createUser
request body:

    {
        "username": "John",
        "password": "this is a strong password"
    }

response body:

    John was added to the MongoDB

## Ερώτημα 2

### Τρόπος υλοποίησης
Αρχικά, με παρόμοιο τρόπο ελέγχουμε αν υπάρχει ήδη το username. Επίσης ελέγχουμε αν το αντοίστοιχο password στην βάση είναι ίδιο με αυτό του request. 

Αν οι προηγούμενοι έλεγχοι ήταν επιτυχείς, καλούμε την create_session()

### Έλεγχος ορθής λειτουργείας
POST request στο http://localhost:5000/login
request body:

    {
        "username": "John",
        "password": "this is a strong password"
    }

response body:

    {
        "uuid": "7d7f87f2-b8f6-11eb-9e0c-436247603927",
        "username": "John"
    }

## Ερώτημα 3

### Τρόπος υλοποίησης
Στην συνάρτηση is_session_valid, περνάμε το uuid που υπάρχει στο header του request, και δρούμε αναλόγως.

Στη συνέχεια αναζητούμε στη ΒΔ με βάση το email του body request, ελέγχοντας ταυτόχρονα για το αν υπάρχει στην βάση ή οχι.

Κατά την εκτύπωση του student, αγνοούμε το πεδίο _id καθώς δεν χρειάζεται.

### Έλεγχος ορθής λειτουργείας
GET request στο http://localhost:5000/getStudent
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

request body:

    {
        "email": "lottieherring@ontagene.com"
    }

response body:

    {
        "name": "Lottie Herring",
        "email": "lottieherring@ontagene.com",
        "yearOfBirth": 1964,
        "address": [
            {
                "street": "Roosevelt Court",
                "city": "Beaulieu",
                "postcode": 13511
            }
        ]
    }

## Ερώτημα 4

### Τρόπος υλοποίησης
Στη μεταβλητή yearLimit αποθηκέυουμε την χρονιά που έχουν γενηθεί όσοι είναι τριάντα φέτος. Επειδή η φετινή χρονιά είναι μεταβλητή ανάλογα το πότε κανείς τρέχει το πρόγραμμα μας, χρησιμοποιούμε την datetime για να βρούμε ποιά είναι αυτή.

Με την χρήση της συνάρτησης find(), ανακτούμε όλους τους student που ταιρίαζουν στο yearLimit που θέσαμε.

Αγνωούμε πάλι το id.

### Έλεγχος ορθής λειτουργείας
GET request στο http://localhost:5000/getStudents/thirties
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

response body:

    [
        {
            "name": "Browning Rasmussen",
            "email": "browningrasmussen@ontagene.com",
            "yearOfBirth": 1991,
            "address": [
                {
                    "street": "Doone Court",
                    "city": "Cuylerville",
                    "postcode": 17331
                }
            ]
        },
        {
            "name": "Bennett Baker",
            "email": "bennettbaker@ontagene.com",
            "yearOfBirth": 1991,
            "gender": "male"
        }
    ]

## Ερώτημα 5

### Τρόπος υλοποίησης
Όμοια με το ερώτημα 4, με την διαφορά ότι χρησιμοποιούμε τον τελστή $lte.

### Έλεγχος ορθής λειτουργείας
GET request στο http://localhost:5000/getStudents/oldies
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

response body:

    [
        {
            "name": "Lavonne Leon",
            "email": "lavonneleon@ontagene.com",
            "yearOfBirth": 1967,
            "address": [
                {
                    "street": "Chauncey Street",
                    "city": "Chase",
                    "postcode": 12663
                }
            ]
        },
        {
            "name": "Patricia Patterson",
            "email": "patriciapatterson@ontagene.com",
            "yearOfBirth": 1979,
            "address": [
                {
                    "street": "Lawn Court",
                    "city": "Bodega",
                    "postcode": 16678
                }
            ]
        },
        {
            "name": "Mcclure Whitfield",
            "email": "mcclurewhitfield@ontagene.com",
            "yearOfBirth": 1971,
            "address": [
                {
                    "street": "Agate Court",
                    "city": "Loveland",
                    "postcode": 12814
                }
            ]
        },
        ...
    ]

## Ερώτημα 6

### Τρόπος υλοποίησης
Αφότου πραγματοποιήσουμε τους ελέγχους, η απάντηση βρίσκεται στο resutls. Το μόνο που μένει είναι να την μορφοποιήσουμε στην μορφή που θέλουμε να το εκτυπώσουμε. Με αυτό τον σκοπό δημιουργούμε το dictionary student, όπου αντιγράφουμε τα στοιχεία που θέλουμε να επιστραφούν.

### Έλεγχος ορθής λειτουργείας
GET request στο http://localhost:5000/getStudentAddress
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

request body:

    {
        "email": "lottieherring@ontagene.com"
    }

response body:

    {
        "name": "Lottie Herring",
        "street": "Roosevelt Court",
        "postcode": 13511
    }

## Ερώτημα 7

### Τρόπος υλοποίησης
Χρήση της delete_one.

### Έλεγχος ορθής λειτουργείας
GET request στο http://localhost:5000/deleteStudent
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

request body:

    {
        "email": "lottieherring@ontagene.com"
    }

response body:

    Lottie Herring was deleted.

## Ερώτημα 8

### Τρόπος υλοποίησης
Με την χρήση της συνάρτησης update_one, και του τελεστή $set, προσθέτουμε το πεδίο courses στον επικαλούμενο student.

### Έλεγχος ορθής λειτουργείας
PATCH request στο http://localhost:5000/addCourses
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

request body:

    {
        "email": "middletondelacruz@ontagene.com",
        "courses" : [
            {"maths" : 10},
            {"InfoSys" : 11},
            {"hard" : 4}
        ]
    }

response body:

    Added student courses successfully.

## Ερώτημα 9

### Τρόπος υλοποίησης
Με την χρήση της συνάρτησης find_one, βρίσκουμε το αντικείμενο του student. Εκεί αναζητούμε τα μαθήματα με βαθμό μεγαλύτερο ίσο με 5, και τα αποθηκεύουμε στο dictionary student.

### Έλεγχος ορθής λειτουργείας
GET request στο http://localhost:5000/getPassedCourses
request header:

    {
    	"authorization": "7d7f87f2-b8f6-11eb-9e0c-436247603927"
    }

request body:

    {
        "email": "middletondelacruz@ontagene.com",
        "courses" : [
            {"maths" : 10},
            {"InfoSys" : 11},
            {"hard" : 4}
        ]
    }

response body:

    {
        "maths": 10,
        "InfoSys": 11
    }



