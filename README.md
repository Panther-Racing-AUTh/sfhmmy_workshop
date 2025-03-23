# Flutter Donation App Workshop on Zapprun

Welcome to the **Flutter Donation App Workshop**! In this workshop, you'll learn how to build a **donation app** using **Flutter** and **Supabase** through **Zapprun**. This guide will walk you through each step of the development process.

---

## 📖 Table of Contents
- [📌 Introduction to Flutter](#-introduction-to-flutter)
- [📌 Introduction to Zapprun](#-introduction-to-zapprun)
- [📌 Introduction to Supabase](#-introduction-to-supabase)
- [⚡ Setting Up Zapprun](#-step-1-setting-up-zapprun)
- [🎨 Building Basic Flutter UI](#-step-2-building-basic-flutter-ui)
- [☁ Setup Supabase](#-step-6-setting-up-supabase)
- [🚀 Running Your App](#-congratulations-)
- [🎯 Conclusion](#-conclusion)
- [🏍 About Panther Racing Auth Team](#%EF%B8%8F-discover-panther-racing-auth)
- [📝 Wikipedia](#-donation-app---wikipedia-style-guide)

---

## 📌 Introduction to Flutter
Flutter is an **open-source UI toolkit** developed by **Google** for building natively compiled applications for **mobile, web, and desktop** from a **single codebase**.

### 🚀 Why Flutter?

✅ **Widgets** – The fundamental building blocks of a Flutter app. Everything is a widget!

✅ **Stateful vs Stateless Widgets**
   - **Stateless Widgets**: Immutable, do not change over time.
   - **Stateful Widgets**: Maintain state, can change dynamically.

✅ **Hot Reload** – Instantly see code changes without restarting the app.

✅ **Material Design** – Pre-designed UI components following Google’s Material Design guidelines.

---
## 📌 Introduction to Zapprun
Zapprun is a **cloud-based development platform** that enables developers to build, test, and deploy Flutter applications **without any local setup**. It provides an **instant coding environment** with all the necessary tools, making development more accessible and collaborative.

💻 **Why Zapprun?**

⚡ **No installation required** – Run Flutter apps without setting up an emulator locally.  
⚡ **Instant setup** – Start coding immediately with a pre-configured environment.  
⚡ **Cloud-based testing** – Test your app in the cloud without performance issues.  
⚡ **Easy collaboration** – Work on projects with your team effortlessly.  

---

## 📌 Introduction to Supabase
Supabase is an **open-source backend-as-a-service (BaaS)** that provides a **PostgreSQL-powered database**, authentication, and real-time capabilities. It allows developers to build scalable applications **without managing complex backend infrastructure**.

☁️ **Why Supabase?**

✅ **Open-source alternative to Firebase** – Own your backend with no vendor lock-in.  
✅ **Built-in authentication** – Secure user authentication out of the box.  
✅ **Database management** – PostgreSQL-powered database with real-time syncing.  
✅ **Serverless functions** – Run backend logic without managing servers.  

---



## ⚡ Step 1: Setting Up Zapprun

### 🛠 Part A: Open Zapprun
1️⃣ **Go to** [Zapprun](https://zapp.run) and **create a new Flutter project**.  
2️⃣ **Name your project** → `donation_app` (or any name you prefer).  
3️⃣ **Wait for initialization** → Zapprun will set up your project automatically.  

---

### 🛠 Part B: Organize Your Folder Structure
Once your project is ready, structure your files as follows:

📂 **Project Directory:** `donation_app/`  
```
📂 donation_app/  
│── 📂 lib/  
│   ├── 📄 main.dart                  # Entry point of the app  
│   ├── 📄 home.dart                  # Home screen UI
│   ├── 📄 leaderboard.dart           # Leaderboard screen UI  
│   ├── 📄 donation_item.dart         # Donation Item Model
│   ├── 📄 donation_item_list.dart    # Donation Item View Class
│   ├── 📄 donator.dart               # Donator Model    
│   ├── 📄 supabase_config.dart       # Supabase connection
│   ├── 📄 functions.dart             # helper functions  
│── 📄 pubspec.yaml                   # Dependencies file  
│── 📂 assets/                        # (Optional) Store images/icons  
```
✅ **`lib/`** → Contains the main application logic and UI components.  
✅ **`pubspec.yaml`** → Manages dependencies and configurations.  
✅ **`assets/`** (Optional) → Store images, icons, or other resources.  

🔹 **Ensure your project follows this structure** to keep your files organized and manageable!  

---


## 🎨 Step 2: Building Basic Flutter UI
We’ll start by creating a simple **Home Page layout**.

### 🛠 Part A: Create `home.dart`
Create a new file named **`home.dart`** and add the following code:

```dart
import 'package:flutter/material.dart';

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Donation App')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'Welcome to the Donation App!',
              style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            Text(
              'Make a difference with your donations.',
              style: TextStyle(fontSize: 18),
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 40),
            ElevatedButton(
              onPressed: () {},
              child: Text('Get Started'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 🛠 Part B: Update `main.dart`
Modify **`main.dart`** to use **HomePage**:

```dart
import 'package:flutter/material.dart';
import 'home.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Donation App',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: HomePage(),
    );
  }
}
```


✅ **Now your Flutter app has a basic UI!**  
🔹 **Next, we’ll structure our code to be ready for Supabase.**  

---

### 🛠 Step 3: Run Your App
Use **Zapprun’s built-in emulator**.  
Run the command:  

```sh
flutter run
```  

---

## 🎨 Step 4: Building a Better UI with Donations List

In this step, we will enhance the basic UI by displaying a list of donations and show a list of donation items with their names in a more structured way. The user will be able to select a donation item to view more details.Also, we will add a leaderboard view for top donors!

### 🛠 Part A: Create a Donation Model

First, create a model class to represent donation items. Create a file called **`donation_item.dart`**:

```dart
class DonationItem {
  final int id;
  final String name;
  final String description;
  double amount;

  DonationItem({
    required this.id,
    required this.name,
    required this.description,
    required this.amount,
  });

  // Convert a DonationItem object to a Map
  Map<String, dynamic> toMap() {
    return {
      'id' : id,
      'name': name,
      'description': description,
      'amount': amount,
    };
  }

  // Create a DonationItem object from a Map
  factory DonationItem.fromMap(Map<String, dynamic> map) {
    return DonationItem(
      id : map['id'] ?? -1,
      name: map['name'] ?? '',
      description: map['description'] ?? '',
      amount: map['amount']?.toDouble() ?? 0.0,
    );
  }
}


```
### 🛠 Part B: Create a Donator Model

Then, create a model class to represent the Donor. Create a file called **`donator.dart`**:

```dart
class Donator {
  final int id;
  final String name;
  final double amountDonated;
  final DateTime donationDate;
  final int donationItemId;

  Donator({
    required this.id,
    required this.name,
    required this.amountDonated,
    required this.donationDate,
    required this.donationItemId
  });

  // Example: Convert Donator to a Map (e.g., for database storage)
  Map<String, dynamic> toMap() {
    return {
      'id' : id,
      'name': name,
      'amount_donated': amountDonated,
      'donation_date': donationDate.toIso8601String(),
      'donation_item_id' : donationItemId
    };
  }

  // Example: Create Donator from a Map (e.g., when retrieving from a database)
  factory Donator.fromMap(Map<String, dynamic> map) {
    return Donator(
      id : map['id'] ?? -1,
      name: map['name'] ?? '',
      amountDonated: map['amount_donated'] ?? 0.0,
      donationDate: DateTime.parse(map['donation_date']),
      donationItemId: map['donation_item_id']
    );
  }
}

```
### 🛠 Part C: Create a list of `Donation Items`

Next, create a new file **`donation_item_list.dart`** where we will create a list view to display the donations. Each item will be clickable, allowing the user to choose where they would like to donate.

```dart
import 'package:flutter/material.dart';
import 'donation_item.dart';

class DonationList extends StatefulWidget {
  final List<DonationItem> donations;

  const DonationList({super.key, required this.donations});

  @override
  State<DonationList> createState() => _DonationListState();
}

class _DonationListState extends State<DonationList> {
  // Function to show the dialog when tapping on a donation item
  Future<void> showCardDialog(BuildContext context, DonationItem donation) {
    return showDialog(
      context: context,
      builder: (context) => AlertDialog(
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(25), // Rounded corners for the dialog with a smoother curve
        ),
        backgroundColor: Colors.white, // White background to keep it clean
        title: Container(
          padding: EdgeInsets.symmetric(vertical: 10),
          decoration: BoxDecoration(
            gradient: LinearGradient(
              colors: [Colors.blueAccent, Colors.purpleAccent], // Gradient background for the title
              begin: Alignment.topLeft,
              end: Alignment.bottomRight,
            ),
            borderRadius: BorderRadius.vertical(top: Radius.circular(25)), // Rounded top corners
          ),
          child: Text(
            donation.name,
            textAlign: TextAlign.center,
            style: TextStyle(
              fontSize: 24,
              fontWeight: FontWeight.bold,
              color: Colors.white, // Title text color
            ),
          ),
        ),
        content: SingleChildScrollView( // Allows scrolling for long content
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              SizedBox(height: 15),
              Text(
                donation.description,
                style: TextStyle(fontSize: 18, color: Colors.black87),
              ),
              SizedBox(height: 20),
              Text(
                'Amount: \$${donation.amount.toStringAsFixed(2)}',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                  color: Colors.green,
                ),
              ),
              SizedBox(height: 30),
              // Donation action button with improved styling
              Center(
                child: ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    primary: Colors.green, // Button color
                    onPrimary: Colors.white, // Text color inside the button
                    padding: EdgeInsets.symmetric(horizontal: 30, vertical: 15), // Button padding
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(20),
                    ),
                    elevation: 10, // Add shadow for depth
                    textStyle: TextStyle(
                      fontSize: 20,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  onPressed: () {
                    // Add your donation action here
                    Navigator.of(context).pop();
                  },
                  child: Text(
                    'Donate Now',
                  ),
                ),
              ),
            ],
          ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              Navigator.of(context).pop();
            },
            child: Text(
              "Close",
              style: TextStyle(
                fontSize: 16,
                color: Colors.blueAccent, // Close button color
                fontWeight: FontWeight.w600,
              ),
            ),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    // Get the width of the screen for responsiveness
    double screenWidth = MediaQuery.of(context).size.width;

    return SizedBox(
      width: screenWidth,
      height: 200,
      child: widget.donations.isEmpty
          // If donation list is empty, show a message
          ? Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    Icons.error_outline,
                    color: Colors.blueAccent,
                    size: 50,
                  ),
                  SizedBox(height: 10),
                  Text(
                    "No Donations Available!",
                    style: TextStyle(
                      fontSize: 18,
                      color: Colors.black54,
                    ),
                  ),
                  SizedBox(height: 5),
                  Text(
                    "Come Back Later!",
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.black87,
                    ),
                  ),
                ],
              ),
            )
          // If donation list is not empty, show the donation items
          : Row(
              children: widget.donations.map((donation) {
                return GestureDetector(
                  onTap: () => showCardDialog(context, donation), // Open dialog on tap
                  child: Card(
                    margin: EdgeInsets.symmetric(vertical: 10, horizontal: 15),
                    elevation: 5,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(15),
                    ),
                    child: Container(
                      width: screenWidth < 600 ? 250 : 350, // Adjust width for smaller screens
                      padding: EdgeInsets.all(15),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Icon(
                            Icons.favorite_border,
                            color: Colors.blueAccent,
                            size: 40,
                          ),
                          SizedBox(height: 10),
                          Text(
                            donation.name,
                            style: TextStyle(
                              fontSize: 22,
                              fontWeight: FontWeight.bold,
                              color: Colors.black87,
                            ),
                          ),
                          SizedBox(height: 5),
                          Text(
                            donation.description,
                            style: TextStyle(fontSize: 16, color: Colors.black54),
                            overflow: TextOverflow.ellipsis, // Prevent overflow
                          ),
                          SizedBox(height: 10),
                          Text(
                            '\$${donation.amount.toStringAsFixed(2)}',
                            style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold, color: Colors.green),
                          ),
                        ],
                      ),
                    ),
                  ),
                );
              }).toList(),
            ),
    );
  }
}
```

### 🛠 Part D: Create a `Leaderboard View`

Next, create a new file **`leaderboard.dart`** where we will create a list view to display each donor's contribution.

```dart
import 'package:flutter/material.dart';
import 'donator.dart';

class Leaderboard extends StatefulWidget {
  final List<Donator> topDonors;

  const Leaderboard({super.key,required this.topDonors});

  @override
  State<Leaderboard> createState() => _LeaderboardState();
}

class _LeaderboardState extends State<Leaderboard> {
  

  // Helper method to get rank color gradients
  List<Color> _rankColors(int rank) {
    switch (rank) {
      case 1:
        return [Colors.amber, Colors.orange];
      case 2:
        return [Colors.grey, Colors.blueGrey];
      case 3:
        return [Colors.brown, Colors.orangeAccent];
      default:
        return [Colors.blueAccent, Colors.blue];
    }
  }

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 5,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(15),
      ),
      child: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Title Section
            Text(
              "Leaderboard",
              style: TextStyle(
                fontSize: 22,
                fontWeight: FontWeight.bold,
                color: Colors.blueAccent,
              ),
            ),
            SizedBox(height: 10),

            // Check if the list is empty
            if (widget.topDonors.isEmpty)
              Center(
                child: Column(
                  children: [
                    Icon(
                      Icons.leaderboard,
                      color: Colors.blueAccent,
                      size: 50,
                    ),
                    SizedBox(height: 10),
                    Text(
                      "No donors yet!",
                      style: TextStyle(
                        fontSize: 18,
                        color: Colors.black54,
                      ),
                    ),
                    SizedBox(height: 5),
                    Text(
                      "Be the first to make a difference!",
                      style: TextStyle(
                        fontSize: 16,
                        color: Colors.black87,
                      ),
                    ),
                  ],
                ),
              )
            else
              // Donors List
              Column(
                children: widget.topDonors.asMap().entries.map((entry) {
                  int rank = entry.key + 1; // Rank starts from 1
                  Donator donor = entry.value; // Using Donator model
                  return Padding(
                    padding: const EdgeInsets.symmetric(vertical: 8.0),
                    child: Row(
                      children: [
                        // Rank Badge
                        Container(
                          width: 40,
                          height: 40,
                          decoration: BoxDecoration(
                            shape: BoxShape.circle,
                            gradient: LinearGradient(
                              colors: _rankColors(rank),
                              begin: Alignment.topLeft,
                              end: Alignment.bottomRight,
                            ),
                          ),
                          child: Center(
                            child: Text(
                              "$rank",
                              style: TextStyle(
                                fontSize: 18,
                                fontWeight: FontWeight.bold,
                                color: Colors.white,
                              ),
                            ),
                          ),
                        ),
                        SizedBox(width: 15),

                        // Donor Details
                        Expanded(
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                donor.name,
                                style: TextStyle(
                                  fontSize: 18,
                                  fontWeight: FontWeight.bold,
                                  color: Colors.black87,
                                ),
                              ),
                              SizedBox(height: 5),
                              Text(
                                'Donated: \$${donor.amountDonated.toStringAsFixed(2)}',
                                style: TextStyle(
                                  fontSize: 16,
                                  color: Colors.green,
                                ),
                              ),
                            ],
                          ),
                        ),

                        // Icon for Celebration
                        Icon(
                          Icons.emoji_events,
                          color: _rankColors(rank)[0],
                        ),
                      ],
                    ),
                  );
                }).toList(),
              ),
          ],
        ),
      ),
    );
  }
}

```


### 🛠 Part E: Update `Home` page to include the Donation List

Now, update your **`home.dart`** to display the `DonationList` & `Leaderboard`. The `HomePage` widget will use the `DonationList` widget to show a list of donation items & `Leaderboard` widget to to show each donor's total contribution ,sorted and ranked.

```dart
import 'package:flutter/material.dart';
import 'donation_item_list.dart';
import 'leaderboard.dart';
import 'donator.dart';
import 'donation_item.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  List<Donator> donators=[];
  List<Donator> topDonators=[];
  List<DonationItem> donationItems=[];

  @override
  void initState() {
    super.initState();
  }
  @override
  void dispose() {
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Donation App'),
        backgroundColor: Colors.blueAccent,
        elevation: 0,
      ),
      backgroundColor: Colors.tealAccent.withOpacity(0.3),

      body: SingleChildScrollView( // Enable vertical scrolling for the entire page
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              // Title section
              Text(
                'Welcome to the Donation App!',
                style: TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                  color: Colors.blueAccent,
                ),
              ),
              SizedBox(height: 10),
              Text(
                'Make a difference with your donations.',
                style: TextStyle(fontSize: 18, color: Colors.black54),
              ),
              SizedBox(height: 30),

              // Donation List title section
              Text(
                'Browse Donations:',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                  color: Colors.blueAccent,
                ),
              ),
              SizedBox(height: 20),

              // Donation List
              DonationList(donations: donationItems),

              SizedBox(height: 15), // Add spacing before the leaderboard
              Divider(),
              SizedBox(height: 15),
              // Leaderboard title section
              Text(
                'Top Donors Leaderboard:',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                  color: Colors.blueAccent,
                ),
              ),
              SizedBox(height: 20),

              // Leaderboard widget
              Leaderboard(topDonors: topDonators),
            ],
          ),
        ),
      ),
    );
  }

}
```


## 🛠 Step 5: Run Your App
Use **Zapprun’s built-in emulator**.  
Run the command:  

```sh
flutter run
```

🚀 Now you're ready to setup our Database!  

---

## ☁ Step 6: Setting Up Supabase

Supabase is an **open-source alternative to Firebase**, providing a powerful backend with authentication, database, and real-time capabilities.

### 🛠 Part A: Create a Supabase Project
1. Go to [Supabase](https://supabase.com) and **sign up** or **log in**.
2. Click **New Project** and enter the project name, database password, and region.
3. Click **Create Project** and wait for the setup to complete.

### 🛠 Part B: Configure Database and API Keys
1. In your **Supabase Dashboard**, go to the **Database** section.
2. Click **Tables** and create a new table named `donators` with the following columns:

   ```plaintext
   id                | int8 (Primary Key, Auto Increment)
   name              | Text (Required)
   amount_donated    | float8 (Required)
   donation_date     | timestamp (Default: now())
   donation_item_id  | int4 (Required,with a foreign key linked to Donation Item ID)
   ```
3. On section **Tables**, create a new table named `donation_items` with the following columns:

   ```plaintext
   id                | int8 (Primary Key, Auto Increment)
   name              | Text (Required)
   description       | Text (Required)
   amount            | float8 (Required)
   ```
4. ❗ **Notice**
   This is the code to build your own database in `Supabase`, compatible with our app!

   ```sql
   -- Create the donation_items table
   CREATE TABLE donation_items (
       id SERIAL PRIMARY KEY,
       name TEXT NOT NULL,
       description TEXT,
       amount FLOAT8 NOT NULL
   );

   -- Create the donators table
   CREATE TABLE donators (
       id SERIAL PRIMARY KEY,
       name TEXT NOT NULL,
       amount_donated FLOAT8 NOT NULL,
       donation_date TIMESTAMPTZ DEFAULT now(),
       donation_item_id INT REFERENCES donation_items(id) ON DELETE SET NULL
   );

   -- Create a view for top donators
   CREATE VIEW top_donators AS
   SELECT 
       name,
       SUM(amount_donated) AS total_donated
   FROM donators
   GROUP BY name
   ORDER BY total_donated DESC;


5. Navigate to **Settings > API** and copy your **Project URL** and **Anon Key**.

---
## 🚨 Warning : `Important Security Notice`

### ⚠️ Supabase Configuration & Security Best Practices

For production environments, **do not** hardcode your Supabase credentials (URL and Anon Key) directly in the source code. Exposing these credentials can lead to security vulnerabilities, such as unauthorized access to your database.

### 💭 **Supabase URL**: This is the **unique** endpoint for your Supabase project, used to interact with the database and services.

### 💭 **Supabase Anon Key**: A public API key that allows **read and write access** based on your project's **authentication rules**. It should be **kept secure in production**.

### 🔐 Secure Implementation
Instead of hardcoding, consider the following best practices:

1. **Use Environment Variables**
   Store your Supabase URL and Anon Key in environment variables:
   
   ```bash
   export SUPABASE_URL="https://your-project.supabase.co"
   export SUPABASE_ANON_KEY="your-anon-key"
   ```
   Then access them in your Dart/Flutter app using:
   
   ```dart
   import 'dart:io';
   
   final String supabaseUrl = Platform.environment['SUPABASE_URL'] ?? '';
   final String supabaseAnonKey = Platform.environment['SUPABASE_ANON_KEY'] ?? '';
   ```

2. **Use a Secrets Manager**
   Services like AWS Secrets Manager, Google Secret Manager, or Firebase Remote Config can help store and manage sensitive credentials securely.

3. **Restrict Supabase Policies**
   Utilize [Row-Level Security (RLS)](https://supabase.com/docs/guides/auth/row-level-security) to enforce access control at the database level.

4. **Configure API Keys with Limited Permissions**
   Avoid using the `service_role` key in client-side applications, as it has full database access.

5. **Monitor API Usage**
   Regularly check your Supabase dashboard for unauthorized requests and set up alerts for suspicious activities.
---

### 🛠 Part C: Download packages
Navigate to `pubspec.yaml` & when you find `dependencies` section, update with this:
```dart
dependencies:
  flutter:
    sdk: flutter
  supabase_flutter: ^1.2.0
  realtime_client: ^1.4.0
```

### 🛠 Part D: Set Up Your Supabase Configuration in Flutter
Inside `supabase_config.dart`, add:

```dart
import 'package:flutter_app/donation_item.dart';
import 'package:flutter_app/donator.dart';
import 'package:supabase_flutter/supabase_flutter.dart';
import 'package:realtime_client/realtime_client.dart';

// Initialize Supabase client using the instance.
final SupabaseClient supabase = Supabase.instance.client;

// Supabase URL and Anon Key (these should be kept safe and not hardcoded in production)
const String supabaseUrl = 'https://xglhodvtllegirsfhsep.supabase.co';
const String supabaseAnonKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhnbGhvZHZ0bGxlZ2lyc2Zoc2VwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDE0MjQyNjgsImV4cCI6MjA1NzAwMDI2OH0.GiAfg_hGCLe2OtWpzKUeG2LUCUxe0LdivZlvBGJ2n98';

// Realtime Client for listening to changes in the database
final RealtimeClient socket = RealtimeClient(
  'wss://xglhodvtllegirsfhsep.supabase.co/realtime/v1',
  params: {'apikey': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InhnbGhvZHZ0bGxlZ2lyc2Zoc2VwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDE0MjQyNjgsImV4cCI6MjA1NzAwMDI2OH0.GiAfg_hGCLe2OtWpzKUeG2LUCUxe0LdivZlvBGJ2n98'},
);

class RealtimeService {
  RealtimeService();

  // Fetch initial data for both donation_items and donators tables
  Future<Map<String, List<Map<String, dynamic>>>> fetchInitialData() async {
    try {
      // Fetch donation items from Supabase
      final donationItemsResponse = await supabase
          .from('donation_items')
          .select()
          .order('id', ascending: true);

      // Fetch donators from Supabase
      final donatorsResponse = await supabase
          .from('donators')
          .select()
          .order('id', ascending: true);

      // Return both fetched lists of donationItems and donators
      return {
        'donationItems': List<Map<String, dynamic>>.from(donationItemsResponse),
        'donators': List<Map<String, dynamic>>.from(donatorsResponse),
      };
    } catch (e) {
      // If there's an error during the fetch, log the error and return empty lists
      print('Error fetching initial data: $e');
      return {'donationItems': [], 'donators': []};
    }
  }

  // Set up real-time listeners for donation_items and donators
  void setupRealtimeListeners({
    required Function(Map<String, dynamic>) onNewDonationItem,
    required Function(Map<String, dynamic>) onUpdateDonationItem,
    required Function(Map<String, dynamic>) onDeleteDonationItem,
    required Function(Map<String, dynamic>) onNewDonator,
    required Function(Map<String, dynamic>) onUpdateDonator,
    required Function(Map<String, dynamic>) onDeleteDonator,
  }) {
    final channel = socket.channel('realtime:public');

    // Listen to INSERT events for donation_items
    channel.on(
      RealtimeListenTypes.postgresChanges,
      ChannelFilter(event: 'INSERT', schema: 'public', table: 'donation_items'),
      (payload, [ref]) => onNewDonationItem(payload['new']),
    );
    channel.on(
      RealtimeListenTypes.postgresChanges,
      ChannelFilter(
        event: 'UPDATE',
        schema: 'public',
        table: 'donation_items',
      ),
      (payload, [ref]) {
        // Ensure 'payload' is of the correct type and contains the 'new' key
        final updatedDonationItem = payload['new'];
        if (updatedDonationItem != null) {
          // Pass the updated data to the 'onUpdateDonationItem' function
          onUpdateDonationItem(updatedDonationItem);
        } else {
          // Handle the case where 'new' is not available
          print('Error: No updated data found in payload');
        }
      },
    );

    // Listen to DELETE events for donation_items
    channel.on(
      RealtimeListenTypes.postgresChanges,
      ChannelFilter(event: 'DELETE', schema: 'public', table: 'donation_items'),
      (payload, [ref]) => onDeleteDonationItem(payload['old']),
    );

    // Listen to INSERT events for donators
    channel.on(
      RealtimeListenTypes.postgresChanges,
      ChannelFilter(event: 'INSERT', schema: 'public', table: 'donators'),
      (payload, [ref]) => onNewDonator(payload['new']),
    );

    // Listen to UPDATE events for donators
    channel.on(
      RealtimeListenTypes.postgresChanges,
      ChannelFilter(
        event: 'UPDATE',
        schema: 'public',
        table: 'donators',
      ),
      (payload, [ref]) {
        // Ensure 'payload' is of the correct type and contains the 'new' key
        final updatedDonator = payload['new'];
        if (updatedDonator != null) {
          // Pass the updated data to the 'onUpdateDonator' function
          onUpdateDonator(updatedDonator);
        } else {
          // Handle the case where 'new' is not available
          print('Error: No updated data found in payload');
        }
      },
    );

    // Listen to DELETE events for donators
    channel.on(
      RealtimeListenTypes.postgresChanges,
      ChannelFilter(event: 'DELETE', schema: 'public', table: 'donators'),
      (payload, [ref]) => onDeleteDonator(payload['old']),
    );

    // Connect and subscribe to the channel
    socket.connect();
    channel.subscribe((state, [_]) => print('SUBSCRIBED with state: $state'));
  }

  // Disconnect the socket
  void disconnect() {
    socket.disconnect();
  }

  // Example function for handling donation item amount updates
  void updateDonationItemAmount({required DonationItem updatedDonationItem}) async {
    final supabaseClient = Supabase.instance.client;
    
    // Update the donation item in the database
    final updateResponse = await supabaseClient.from('donation_items').update({
      'amount': updatedDonationItem.amount,
    }).eq('id', updatedDonationItem.id);

    // Optionally handle the response, check for errors, etc.
  }
}

// Function to add a new donator to Supabase
Future<void> addDonatorToSupabase({
  required String name,
  required double amountDonated,
  required DateTime donationDate,
  required int donationItemId,
}) async {
  final client = Supabase.instance.client;

  try {
    // Insert the new donator record into the donators table
    final response = await client
        .from('donators') // Specify the table name
        .insert([
          {
            'name': name,
            'amount_donated': amountDonated,
            'donation_date': donationDate.toIso8601String(),
            'donation_item_id': donationItemId,
          }
        ]);

    // Optionally handle the response, check for errors, etc.
  } catch (e) {
    // Handle any errors during the insertion process
    print('Error adding donator: $e');
  }
}

```

### 🛠 Part E: Add necessary `Functions`
Inside `functions.dart`, add:

```dart
import 'package:flutter_app/donation_item.dart';
import 'package:flutter_app/donator.dart';
import 'package:flutter_app/supabase_config.dart';

bool calculateDonationItemTotals({
  required List<Donator> donators,
  required List<DonationItem> donationItems,
}) {
  if (donators.isEmpty || donationItems.isEmpty) return false;

  // Reset all amounts raised in donationItems
  for (var donationItem in donationItems) {
    donationItem.amount = 0.0;
  }

  // Calculate totals
  for (var donator in donators) {
    for (var donationItem in donationItems) {
      if (donator.donationItemId == donationItem.id) {
        donationItem.amount += donator.amountDonated;
      }
    }
  }
  for (var donationItem in donationItems) {
    RealtimeService().updateDonationItemAmount(updatedDonationItem: donationItem);
  }

  return true; // Indicate the operation was successful
}
  List<Donator> groupAndSortDonators(List<Donator> donators) {
  // Group donators by name
  final Map<String, Donator> groupedDonators = {};

  for (var donator in donators) {
    // Ensure the donator object is not null
    if (donator == null || donator.name.isEmpty) continue;

    if (groupedDonators.containsKey(donator.name)) {
      // If the name exists, sum up the donations
      final existingDonator = groupedDonators[donator.name]!;

      groupedDonators[donator.name] = Donator(
        id: existingDonator.id,
        name: existingDonator.name,
        amountDonated: existingDonator.amountDonated + donator.amountDonated,
        donationDate: existingDonator.donationDate,
        donationItemId: existingDonator.donationItemId,
      );
    } else {
      // Add the donator to the map
      groupedDonators[donator.name] = donator;
    }
  }

  // Convert the map values to a list and sort by amountDonated in descending order
  final List<Donator> groupedAndSortedDonators = groupedDonators.values.toList();
  groupedAndSortedDonators.sort((a, b) => b.amountDonated.compareTo(a.amountDonated));

  return groupedAndSortedDonators;
}
```
### 🛠 Part F: Update `Main` page

Now, let's establish the `initialization of Supabase` inside `main.dart` before running the app.

```dart
import 'package:flutter/material.dart';
import 'home.dart';
import 'package:supabase_flutter/supabase_flutter.dart';
import 'package:flutter_app/supabase_config.dart';

void main() async{
  WidgetsFlutterBinding.ensureInitialized();

  await Supabase.initialize(
    url: supabaseUrl,
    anonKey: supabaseAnonKey,
  );
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Donation App',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: HomePage(),
    );
  }
}
```
### 🛠 Part G: Update  `Home` page 

Last step before running the app, we have to update your **`home.dart`**,by adding `loadInitialData()` & `setupListeners()` functions.
 

Update **`home.dart`** with the following code:

```dart
import 'package:flutter/material.dart';
import 'package:supabase_flutter/supabase_flutter.dart';
import 'supabase_config.dart';
import 'package:flutter_app/donation_item_list.dart';
import 'package:flutter_app/leaderboard.dart';
import 'package:flutter_app/donator.dart';
import 'package:flutter_app/donation_item.dart';
import 'package:flutter_app/functions.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  late RealtimeService realtimeService=RealtimeService();
  
  List<Donator> donators = [];
  List<Donator> topDonators=[];
  List<DonationItem> donationItems=[];
   @override
  void initState() {
    super.initState();

    
    loadInitialData();
    setupListeners();
  }
  @override
  void dispose() {
    socket.disconnect();
    super.dispose();
  }

   Future<void> loadInitialData() async {
    final initialData = await realtimeService.fetchInitialData();
    donationItems = List<Map<String, dynamic>>.from(initialData['donationItems'] ?? [])
        .map((item) => DonationItem.fromMap(item))
        .toList();

      donators = List<Map<String, dynamic>>.from(initialData['donators'] ?? [])
        .map((item) => Donator.fromMap(item))
        .toList();
        
      topDonators = groupAndSortDonators(donators);
      setState(() {
        
      });
  }

 
  void setupListeners() {
    realtimeService.setupRealtimeListeners(
      onNewDonationItem: (newItem) {
        setState(() {
          donationItems.add(DonationItem.fromMap(newItem));
        });
      },
      onUpdateDonationItem: (updatedItem) {
        setState(() {
          int index = donationItems.indexWhere((element) => element.id == DonationItem.fromMap(updatedItem).id,);

          donationItems[index] = DonationItem.fromMap(updatedItem);
        });
      },
      onDeleteDonationItem: (deletedItem) {
        setState(() {
          donationItems.removeWhere((item) => item.name == DonationItem.fromMap(deletedItem).name);
        });
      },
      onNewDonator: (newDonator) {
        setState(() {
          donators.add(Donator.fromMap(newDonator));
          topDonators = groupAndSortDonators(donators);
          calculateDonationItemTotals(donators: donators,donationItems: donationItems);
        });
      },
      onUpdateDonator: (updatedDonor) {
        setState(() {
          int index = donators.indexWhere((element) => element.id == Donator.fromMap(updatedDonor).id,);

          donators[index] = Donator.fromMap(updatedDonor);
          topDonators = groupAndSortDonators(donators);
          calculateDonationItemTotals(donators: donators,donationItems: donationItems);


        });
      },
      onDeleteDonator: (deletedDonator) {
        setState(() {
          donators.removeWhere((item) => item.name == Donator.fromMap(deletedDonator).name);
        });
      },
    );
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Donation App'),
        backgroundColor: Colors.blueAccent,
        elevation: 0,
      ),
      backgroundColor: Colors.tealAccent.withOpacity(0.3),
      body: SingleChildScrollView( // Enable vertical scrolling for the entire page
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              // Title section
              Text(
                'Welcome to the Donation App!',
                style: TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                  color: Colors.blueAccent,
                ),
              ),
              SizedBox(height: 10),
              Text(
                'Make a difference with your donations.',
                style: TextStyle(fontSize: 18, color: Colors.black54),
              ),
              SizedBox(height: 30),

              // Donation List title section
              Text(
                'Browse Donations:',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                  color: Colors.blueAccent,
                ),
              ),
              SizedBox(height: 20),

              // Donation List
              DonationList(donations: donationItems),

              SizedBox(height: 15), // Add spacing before the leaderboard
              Divider(),
              SizedBox(height: 15),
              // Leaderboard title section
              Text(
                'Top Donors Leaderboard:',
                style: TextStyle(
                  fontSize: 22,
                  fontWeight: FontWeight.bold,
                  color: Colors.blueAccent,
                ),
              ),
              SizedBox(height: 20),

              // Leaderboard widget
              Leaderboard(topDonors: topDonators),
            ],
          ),
        ),
      ),
    );
  }

}
```



---

# 🎉 **Congratulations!** 🎉

**You've successfully built your Donation App with Supabase and Flutter!** 🚀

---

### 🏆 **What's Next?**

Feel free to **explore** and **play** with the app.  
You can start:
- Donating 💰
- Viewing **real-time data** 🔄

The setup is complete, and now the **fun part** begins! 🎉

---

### 🎯 **What to Do Next?**

- **Enjoy** building, testing, and expanding your app!  
- **The possibilities are endless!** 🌟

### 💡 **Keep pushing your ideas forward and make it your own!**

---

## 🎯 Conclusion

🎉 **Congratulations!** You've successfully built a **Donation App** using **Flutter** and **Supabase** with the help of **Zapprun**. This marks an exciting milestone, but your journey doesn't have to stop here!

### 🚀 Next Steps

- 🔹 **Enhance Your App:** Implement features like user authentication, analytics, or recurring donations.
- 🎨 **Improve UI/UX:** Experiment with animations, dark mode, or custom themes for a better user experience.
- ⚡ **Optimize Performance:** Utilize caching, lazy loading, and API optimizations to make your app faster.

💡 Keep refining your app, exploring new tools, and pushing the boundaries of what you can create! 

🚀 **Happy Coding & Keep Innovating!** ✨  

---
---

## 🏍️ **Discover Panther Racing AUTh**  

Panther Racing AUTh is a dynamic team of **Mechanical Engineering students** from the **Aristotle University of Thessaloniki (AUTh)**. Since 2017, the team has been innovating and excelling in designing and building **high-performance Moto3-type racing motorcycles**, proudly representing Greece on international stages.  

🌐 **Explore their journey and achievements**:  
[![Visit Website](https://img.shields.io/badge/🌍-Visit_Official_Website-blue?style=for-the-badge)](https://pantherauth.gr/?utm_source=chatgpt.com)  

---

### 🏆 **Our Mission**  

At Panther Racing AUTh, we strive to:  
- 🛠️ **Innovate Motorsport Engineering**: Redefining what student engineers can achieve.  
- 🚀 **Inspire Future Engineers**: Offering hands-on experience to bridge theory and real-world application.  
- 🌍 **Compete Globally**: Representing Greece in the prestigious **MotoStudent competition** in Spain.  

---

### 🔧 **Our Projects**  

- **Panthera Pardus**: Our most advanced racing motorcycle yet, merging cutting-edge technology with performance.
- **Black Panther**: A giant leap forward in engineering and design.
- **Pink Panther**: The pioneering project that started it all.
  
---

### 🌟 **Key Achievements**  

🏁 **MotoStudent Success**  
Representing AUTh and Greece at an international competition where innovation meets speed.  

⚙️ **Dynamic Velocity Stack System**  
A breakthrough in racing performance, adjusting dynamically for race conditions.  

📱 **Velocity-Handler App**  
A Flutter-powered mobile app for real-time RPM monitoring and data analysis.  

---

### 🌍 **Join the Panther Racing AUTh Community**  

Stay connected and follow our exciting journey:  

[![Official Website](https://img.shields.io/badge/🌐-Official_Website-blue?style=for-the-badge)](https://pantherauth.gr/?utm_source=chatgpt.com)  
[![YouTube Channel](https://img.shields.io/badge/🎥-Subscribe_to_YouTube-red?style=for-the-badge)](https://www.youtube.com/channel/UCnCk6y5iHD9FkplzY4gyLtA/about)  
[![LinkedIn Profile](https://img.shields.io/badge/💼-Connect_on_LinkedIn-blue?style=for-the-badge)](https://www.linkedin.com/company/panther-racing-auth/)  
[![GitHub Repository](https://img.shields.io/badge/👨‍💻-Explore_on_GitHub-black?style=for-the-badge)](https://github.com/Panther-Racing-AUTh?utm_source=chatgpt.com)

---

### 🎉 **Thank You for Supporting Us!**  

By supporting Panther Racing AUTh, you join a community that values passion, innovation, and excellence. Let’s create something extraordinary together!  

---


👉 **Join the movement today!**  
Explore, engage, and be part of Panther Racing AUTh’s mission to redefine motorsport engineering. Together, we can achieve greatness. 🚀  

---
---


# 📝 Donation App - Wikipedia-style Guide

---

## 1. Flutter UI Widgets Used

### 1.1. `Scaffold`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Scaffold` widget is a high-level layout widget that provides the basic structure for your app's visual interface. It includes app bars, floating action buttons, drawers, and more.  
- **Key Properties**:
  - `appBar`: A widget to display the app's header.
  - `body`: The primary content of the screen.
  - `floatingActionButton`: A button that "floats" above the body content.
- **Usage in App**: Used to structure main pages, including the home screen and donation list page.

---

### 1.2. `AppBar`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `AppBar` widget displays a horizontal bar at the top of the screen. It is typically used to show titles, navigation buttons, and actions.  
- **Key Properties**:
  - `title`: A widget, usually a `Text`, to display the app's title.
  - `actions`: A list of widgets, such as buttons, displayed on the right side of the bar.
- **Usage in App**: Used to display the app's title and action buttons like navigation or settings.

---

### 1.3. `ListView`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: A `ListView` is a scrollable list of widgets. It is ideal for displaying data in rows or items dynamically.  
- **Key Properties**:
  - `children`: A list of widgets to display.
  - `builder`: A function that creates widgets lazily based on the index.
- **Usage in App**: Used to display lists of donation items and donators.

---

### 1.4. `Text`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Text` widget displays a string of text with a single style. It is used to show static or dynamic content.  
- **Key Properties**:
  - `style`: A `TextStyle` object that defines the font size, color, weight, etc.
  - `textAlign`: Aligns the text horizontally within its parent.
- **Usage in App**: Used to display labels, titles, and descriptions throughout the UI.

---

### 1.5. `ElevatedButton`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `ElevatedButton` is a material design button that lifts slightly when pressed.  
- **Key Properties**:
  - `onPressed`: A callback function triggered when the button is pressed.
  - `child`: The widget inside the button, usually a `Text` or `Icon`.
- **Usage in App**: Used to handle user actions like submitting donations or navigating between pages.

---

### 1.6. `Icon`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Icon` widget is used to display material design icons.  
- **Key Properties**:
  - `icon`: An instance of an `IconData` object, such as `Icons.add`.
  - `color`: The color of the icon.
  - `size`: The size of the icon in logical pixels.
- **Usage in App**: Used in action buttons and headers to visually indicate actions.

---

### 1.7. `Column`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: A `Column` widget arranges its children vertically.  
- **Key Properties**:
  - `children`: A list of widgets to display vertically.
  - `mainAxisAlignment`: Defines how the children are aligned along the main axis.
  - `crossAxisAlignment`: Defines how the children are aligned along the cross axis.
- **Usage in App**: Used to create vertically stacked layouts, such as form inputs and details views.

---

### 1.8. `Row`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: A `Row` widget arranges its children horizontally.  
- **Key Properties**:
  - `children`: A list of widgets to display horizontally.
  - `mainAxisAlignment`: Defines how the children are aligned along the main axis.
  - `crossAxisAlignment`: Defines how the children are aligned along the cross axis.
- **Usage in App**: Used to create horizontal layouts, such as displaying a label and its value side by side.

---

### 1.9. `FloatingActionButton`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `FloatingActionButton` is a circular button that "floats" above the UI and is commonly used for primary actions.  
- **Key Properties**:
  - `onPressed`: A callback function triggered when the button is pressed.
  - `child`: The widget inside the button, usually an `Icon`.
- **Usage in App**: Used for adding new donation items or donators.

---

### 1.10. `Snackbar`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: A `Snackbar` is a temporary message displayed at the bottom of the screen.  
- **Key Properties**:
  - `content`: A widget, usually `Text`, that displays the message.
  - `action`: An optional action button to provide a quick response to the message.
- **Usage in App**: Used to show confirmation messages like "Donation added successfully."

---

### 1.11. `Container`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Container` widget is a versatile container for styling and positioning child widgets.  
- **Key Properties**:
  - `color`: The background color of the container.
  - `padding`: Adds space inside the container around its child.
  - `margin`: Adds space outside the container.
  - `decoration`: Allows for adding borders, gradients, and rounded corners.
- **Usage in App**: Used for creating visually distinct sections or spacing between elements.

---

### 1.12. `Card`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Card` widget provides a rectangular container with rounded corners and elevation, used for grouping related content.  
- **Key Properties**:
  - `elevation`: Adds shadow to the card.
  - `child`: The widget inside the card, usually a `Column` or `ListTile`.
- **Usage in App**: Used to display information about donation items or donors in a visually appealing format.

---

### 1.13. `GestureDetector`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `GestureDetector` widget detects user interactions, such as taps, swipes, and drags.  
- **Key Properties**:
  - `onTap`: A callback triggered when the widget is tapped.
  - `onLongPress`: A callback triggered on a long press.
- **Usage in App**: Used for detecting user interactions, such as tapping on a list item to view details.

---

### 1.14. `Stack`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Stack` widget allows for overlapping its children, which can be positioned using the `Positioned` widget.  
- **Key Properties**:
  - `alignment`: Aligns the stack's children.
  - `children`: A list of widgets to display.
- **Usage in App**: Used to layer widgets, such as displaying a floating badge over an image.

---

### 1.15. `Positioned`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Positioned` widget is used inside a `Stack` to control the position of a child relative to the stack's edges.  
- **Key Properties**:
  - `top`, `left`, `right`, `bottom`: Position offsets relative to the stack.
- **Usage in App**: Used to position badges or labels over donation images.

---

### 1.16. `GridView`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `GridView` widget displays items in a scrollable, grid-based layout.  
- **Key Properties**:
  - `gridDelegate`: Defines the grid structure, such as the number of columns.
  - `children`: A list of widgets to display in the grid.
- **Usage in App**: Could be used to display donation items in a visually appealing grid format.

---

### 1.17. `Form`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Form` widget groups input fields together and manages their validation and submission.  
- **Key Properties**:
  - `key`: A `GlobalKey` used to identify and validate the form.
  - `child`: The widget tree containing the form fields.
- **Usage in App**: Used to create forms for adding new donation items or donor details.

---

### 1.18. `TextFormField`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `TextFormField` widget is a form-ready text input field with validation support.  
- **Key Properties**:
  - `controller`: A `TextEditingController` to retrieve the input value.
  - `decoration`: Defines the input field's appearance, such as a label or hint.
  - `validator`: A function for input validation.
- **Usage in App**: Used for user input, such as entering donor names or donation amounts.

---

### 1.19. `IconButton`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `IconButton` widget displays an interactive icon that triggers a function when tapped.  
- **Key Properties**:
  - `icon`: An `Icon` widget to display.
  - `onPressed`: A callback triggered when the button is tapped.
- **Usage in App**: Used for quick actions like deleting or editing donation items.

---

### 1.20. `DropdownButton`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `DropdownButton` widget allows users to select an item from a dropdown menu.  
- **Key Properties**:
  - `items`: A list of `DropdownMenuItem` widgets to display.
  - `value`: The currently selected item.
  - `onChanged`: A callback triggered when the user selects an item.
- **Usage in App**: Used to select predefined options, such as donation categories.

---

### 1.21. `Checkbox`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `Checkbox` widget allows users to toggle between two states: checked and unchecked.  
- **Key Properties**:
  - `value`: The current state of the checkbox.
  - `onChanged`: A callback triggered when the checkbox state changes.
- **Usage in App**: Used for toggling options or filtering data.

---

### 1.22. `ProgressIndicator`
- **Type**: Widget  
- **Package**: `flutter/material.dart`  
- **Description**: The `ProgressIndicator` widget displays a visual indicator for ongoing tasks, such as loading data.  
- **Variants**:
  - `CircularProgressIndicator`: Displays a circular loading spinner.
  - `LinearProgressIndicator`: Displays a linear loading bar.
- **Usage in App**: Used during API calls to indicate loading.

---

## 2. Functions and Database Integration

Refer to the "Functions Used" section above for detailed information on backend-related functions.


### 2.1. `SupabaseClient`
- **Type**: Class  
- **Package**: `supabase_flutter`  
- **Description**: `SupabaseClient` is the main class used to interact with the Supabase service. It allows you to perform CRUD operations on your Supabase database, authenticate users, and manage storage and real-time capabilities. In this app, the `SupabaseClient` is used to fetch donation items and donators' data from the Supabase database.

---

### 2.2. `RealtimeClient`
- **Type**: Class  
- **Package**: `realtime_client`  
- **Description**: `RealtimeClient` is used to establish real-time communication with a Supabase database. This allows the app to listen for changes such as inserts, updates, and deletes in real-time. The `RealtimeClient` is responsible for maintaining a websocket connection to the Supabase instance, which allows you to receive updates whenever data changes in the database.

---

### 2.3. `Channel`
- **Type**: Class  
- **Package**: `realtime_client`  
- **Description**: A `Channel` represents a subscription to a specific set of events in the Supabase real-time system. It allows you to listen for specific events (e.g., INSERT, UPDATE, DELETE) on a given table in the database. Channels are used in conjunction with the `RealtimeClient` to subscribe to specific real-time updates for tables like `donation_items` and `donators`.

---

### 2.4. `ChannelFilter`
- **Type**: Class  
- **Package**: `realtime_client`  
- **Description**: `ChannelFilter` is used to filter specific events for a particular table in the Supabase database. It allows you to specify which type of operation (INSERT, UPDATE, DELETE) and which schema and table the real-time listener should be applied to. This is useful to narrow down real-time notifications to relevant changes in the app.

---

### 2.5. `RealtimeListenTypes`
- **Type**: Enum  
- **Package**: `realtime_client`  
- **Description**: `RealtimeListenTypes` defines the types of real-time events that can occur in Supabase. It includes events like `postgresChanges` which triggers when there are changes to a table in the database, such as INSERT, UPDATE, or DELETE.

---

## 3. Functions Used

### 3.1. `fetchInitialData`
- **Type**: Function  
- **Parameters**: None  
- **Return Type**: `Future<Map<String, List<Map<String, dynamic>>>>`  
- **Description**: This function fetches the initial data for the `donation_items` and `donators` tables from Supabase. It makes use of the `SupabaseClient` to query the database and retrieve the data, which is then returned in a map format. If an error occurs, it catches the exception and returns empty lists.

---

### 3.2. `setupRealtimeListeners`
- **Type**: Function  
- **Parameters**: 
  - `onNewDonationItem`: A callback function that is triggered when a new donation item is inserted into the database.
  - `onUpdateDonationItem`: A callback function that is triggered when a donation item is updated in the database.
  - `onDeleteDonationItem`: A callback function that is triggered when a donation item is deleted from the database.
  - `onNewDonator`: A callback function that is triggered when a new donator is inserted into the database.
  - `onUpdateDonator`: A callback function that is triggered when a donator's information is updated in the database.
  - `onDeleteDonator`: A callback function that is triggered when a donator is deleted from the database.
- **Return Type**: `void`  
- **Description**: This function sets up real-time listeners for both `donation_items` and `donators` tables. It subscribes to events like `INSERT`, `UPDATE`, and `DELETE` for each table. Whenever a change is detected, it invokes the corresponding callback functions. These listeners keep the app up-to-date with changes in real time.

---

### 3.3. `disconnect`
- **Type**: Function  
- **Parameters**: None  
- **Return Type**: `void`  
- **Description**: This function disconnects the real-time socket connection. It is typically called when the app no longer needs to listen to real-time changes, such as when the user exits the app or navigates away from the page that requires real-time data.

---

### 3.4. `updateDonationItemAmount`
- **Type**: Function  
- **Parameters**: 
  - `updatedDonationItem`: An object of type `DonationItem` that contains the updated donation item data.
- **Return Type**: `Future<void>`  
- **Description**: This function is used to update the `amount` of a specific donation item in the `donation_items` table. It takes the updated data and sends a request to the Supabase database to update the corresponding row.

---

### 3.5. `addDonatorToSupabase`
- **Type**: Function  
- **Parameters**: 
  - `name`: The name of the donator.
  - `amountDonated`: The amount donated by the donator.
  - `donationDate`: The date the donation was made.
  - `donationItemId`: The ID of the donation item associated with the donation.
- **Return Type**: `Future<void>`  
- **Description**: This function is used to insert a new donator's information into the `donators` table in the Supabase database. It takes the donator's details as parameters, formats them as a map, and inserts the record into the database.

---

## 4. Database Tables and Structure

### 4.1. `donation_items` Table
- **Description**: This table holds information about the items available for donation in the app. It includes the following columns:
  - `id`: The unique identifier for each donation item (Primary Key, Auto Increment).
  - `name`: The name of the donation item (e.g., "Food Package", "Clothes").
  - `description`: A brief description of the donation item.
  - `amount`: The monetary value of the donation item.

---

### 4.2. `donators` Table
- **Description**: This table stores information about the donators, including the donation amount and associated donation item. It includes the following columns:
  - `id`: The unique identifier for each donator (Primary Key, Auto Increment).
  - `name`: The name of the donator.
  - `amount_donated`: The total amount donated by the donator.
  - `donation_date`: The date when the donation was made.
  - `donation_item_id`: A foreign key that references the ID of the donation item the donator contributed to.

---

## 5. Summary

This app leverages Flutter for the frontend and Supabase for the backend. With features such as real-time updates, the app enables users to see live changes when donation items or donators are added, updated, or deleted. The functions, such as `fetchInitialData`, `setupRealtimeListeners`, and `addDonatorToSupabase`, interact with the Supabase database to handle CRUD operations and maintain real-time sync.

The app structure is designed to keep the user experience seamless and up-to-date, providing a real-time dashboard for users to track donations and items.

---

This Wikipedia-style guide provides a structured overview of the widgets and functions used in the app, ensuring you have a clear understanding of each component.
