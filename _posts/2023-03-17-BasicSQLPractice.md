---
layout: single
title: "SQL Practice"
categories: "school-project"
tag: [sql, php]
toc: true
toc_sticky: true
---

Basic SQL Query Practice with PHP files

## Question

![p1]({{site.url}}/images/2023-03-17-BasicSQLPractice/p1.png)
![p2]({{site.url}}/images/2023-03-17-BasicSQLPractice/p2.png)
![p3]({{site.url}}/images/2023-03-17-BasicSQLPractice/p3.png)

## Sample Data

```sql
INSERT INTO Member (member_ID, name, email, contact_number) VALUES
(1, 'John Doe', 'johndoe@gmail.com', '+1-555-555-5555'),
(2, 'Tom Xie', 'tomxie@gmail.com', '+1-555-555-5556'),
(3, 'John Smith', 'johnsmith@gmail.com', '+1-555-555-5584'),
(4, 'Ava Rodriguez', 'arodriguez@gmail.com', '+1-555-555-5555'),
(5, 'Ethan Davis', 'edavis@yahoo.com', '+1-555-555-5556'),
(6, 'Isabella Lee', 'ilee@hotmail.com', '+1-555-555-5557'),
(7, 'Oliver Nguyen', 'onguyen@gmail.com', '+1-555-555-5558'),
(8, 'Sophia Kim', 'skim@yahoo.com', '+1-555-555-5559'),
(9, 'William Chen', 'wchen@hotmail.com', '+1-555-555-5560'),
(10, 'Mia Thompson', 'mthompson@gmail.com', '+1-555-555-5561'),
(11, 'Michael Martinez', 'mmartinez@yahoo.com', '+1-555-555-5562'),
(12, 'Charlotte Wilson', 'cwilson@hotmail.com', '+1-555-555-5563'),
(13, 'David Brown', 'dbrown@gmail.com', '+1-555-555-5564'),
(14, 'Harper Lee', 'hlee@yahoo.com', '+1-555-555-5565'),
(15, 'Daniel Hernandez', 'dhernandez@hotmail.com', '+1-555-555-5566'),
(16, 'Avery Collins', 'acollins@gmail.com', '+1-555-555-5567'),
(17, 'Natalie Baker', 'nbaker@yahoo.com', '+1-555-555-5568'),
(18, 'Alexander Chen', 'achen@hotmail.com', '+1-555-555-5569'),
(19, 'Evelyn Kim', 'ekim@gmail.com', '+1-555-555-5570'),
(20, 'Liam Clark', 'lclark@yahoo.com', '+1-555-555-5571'),
(21, 'Aaliyah Lee', 'alee@hotmail.com', '+1-555-555-5572'),
(22, 'Lucas Jones', 'ljones@gmail.com', '+1-555-555-5573'),
(23, 'Madison Lee', 'mlee@yahoo.com', '+1-555-555-5574'),
(24, 'Jacob Hernandez', 'jhernandez@hotmail.com', '+1-555-555-5575'),
(25, 'Aurora Wilson', 'awilson@gmail.com', '+1-555-555-5576'),
(26, 'Benjamin Kim', 'bkim@yahoo.com', '+1-555-555-5577'),
(27, 'Violet Brown', 'vbrown@hotmail.com', '+1-555-555-5578'),
(28, 'Avery Martinez', 'amartinez@gmail.com', '+1-555-555-5579'),
(29, 'Kelly Yuen', 'yuenkeylly@gmail.com', '+1-555-598-09918');


INSERT INTO ServiceArea (service_area_ID, name, parent_area_ID) VALUES
(1, 'Hong Kong Island', null),
(2, 'Sha Tin', null),
(3, 'Kowloon', null),
(4, 'NT', null),
(5, 'Central and Western', 1),
(6, 'Eastern', 1),
(7, 'Southern', 1),
(8, 'Pokfulam', 5),
(9, 'West Kowloon Reclamation', 3),
(10, 'Yau Tsim Mong', 3),
(11, 'Kowloon Tong', 3),
(12, 'Clear Water Bay', 4),
(13, 'North Point', 1);


INSERT INTO Locker (locker_ID, service_area_ID, name) VALUES
(1, 8, 'HKU Station Locker'),
(2, 8, 'Pofulam Convenient Store Locker'),
(3, 8, 'Pofulam Gas Station Locker'),

(4, 12, 'HKSTP Locker A'),
(5, 12, 'HKSTP Locker B'),
(6, 12, 'HKSTP Locker C'),

(7, 2, 'CUHK Locker A'),
(8, 2, 'CUHK Locker B'),
(9, 2, 'HKUST Locker A'),

(10, 12, 'HKUST Locker B'),
(11, 12, 'HKK Locker A'),
(12, 12, 'HKK Locker B'),
(13, 12, 'HKK Locker C'),
(14, 12, 'HKK Locker D'),
(15, 12, 'HKK Locker E'),

(16, 13, 'North Point Locker A'),
(17, 13, 'North Point Locker B'),
(18, 13, 'North Point Locker C'),
(19, 13, 'North Point Locker D'),
(20, 13, 'North Point Locker E'),

(21, 9, 'West Kowloon Reclamation A'),
(22, 9, 'West Kowloon Reclamation B'),
(23, 9, 'West Kowloon Reclamation C'),
(24, 9, 'West Kowloon Reclamation D'),
(25, 9, 'West Kowloon Reclamation E'),
(26, 9, 'West Kowloon Reclamation F'),
(27, 9, 'West Kowloon Reclamation G'),
(28, 9, 'West Kowloon Reclamation H');


INSERT INTO GroupOrder (order_ID, member_ID) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7), 
(8, 9), 
(9, 8),
(10, 11),
(11, 12), 
(12, 13),
(13, 15),

(14, 9),
(15, 10),

(16, 11),
(17, 12),
(18, 13),
(19, 14),
(20, 15),
(21, 16),
(22, 17),
(23, 18),
(24, 21),
(25, 21), 
(26, 21),
(27, 21),
(28, 21),
(29, 6), 
(30, 6),
(31, 6),
(32, 6),
(33, 6),
(34, 6),
(35, 6);




INSERT INTO Package (package_ID, owner_ID, order_ID, weight) VALUES
(1, 1, 1, 2),
(2, 2, 2, 10),
(3, 3, 3, 11),
(4, 4, 4, 13),
(5, 5, 5, 15),
(6, 6, 6, 21),

(7, 1, 1, 1),
(8, 2, 2, 6),
(9, 3, 3, 9),
(10, 4, 4, 10),
(11, 5, 5, 9),
(12, 6, 6, 3),

(13, 8, 9, 9),
(14, 8, 9, 8),
(15, 8, 9, 1),



(16, 9, 14, 2),
(17, 10, 15, 3),
(18, 11, 16, 2),
(19, 12, 17, 19),
(20, 13, 18, 83),
(21, 14, 19, 91),
(22, 15, 20, 92),
(23, 16, 21, 91),
(24, 17, 22, 20),
(25, 18, 23, 21),


(26, 21, 25, 23),
(27, 21, 26, 6),
(28, 21, 27, 9),
(29, 21, 28, 11),
(30, 21, 25, 13),
(31, 21, 26, 9),
(32, 21, 27, 10),
(33, 21, 28, 6),

(34, 6, 30, 8),
(35, 6, 31, 9),
(36, 6, 32, 10),
(37, 6, 33, 15),
(38, 6, 34, 21),
(39, 6, 35, 9),

(40, 6, 30, 2),
(41, 6, 31, 3),
(42, 6, 32, 6),
(43, 6, 33, 8),
(44, 6, 34, 8),
(45, 6, 35, 8),

(46, 6, 30, 10),
(47, 6, 31, 11),
(48, 6, 32, 12),
(49, 6, 33, 9),
(50, 6, 34, 2),
(51, 6, 35, 3),


(52, 1, 1, 6),
(53, 1, 1, 12),
(54, 1, 1, 13),
(55, 1, 1, 20),
(56, 1, 1, 2),

(57, 2, 2, 3),
(58, 2, 2, 2),

(59, 3, 3, 1),


(60, 11, 16, 2),
(61, 11, 16, 2),
(62, 12, 17, 3),
(63, 13, 18, 3),
(64, 14, 19, 10),
(65, 15, 20, 11);


INSERT INTO HomeDeliveryOrder (order_ID, address) VALUES
(7, 'CB320, KU, Central and Western, Hong Kong Island'),
(8, 'WK321, KU, central and western, Hong Kong Island'),
(10, '456 Elm St.'),
(11, 'X293, KU, Central and Western, Hong Kong Island'),
(12, 'X920, KU, Central and Western, Hong Kong Island'),
(13, 'CH9283, KU, Central and Western, Hong Kong Island');


INSERT INTO LockerPickupOrder (order_ID, locker_ID, cell_number, arrival_datetime, collect_datetime) VALUES
(1, 1, 1, '2023-02-16 12:00:00', null),
(2, 2, 1, '2023-02-15 12:00:00', '2023-02-21 13:00:00'),
(3, 3, 1, '2023-02-19 12:00:00', '2023-02-22 13:00:00'),
(4, 1, 1, '2023-02-10 12:00:00', null),
(5, 2, 1, '2023-02-19 12:00:00', null),
(6, 3, 2, '2023-02-19 12:00:00', '2023-02-23 13:00:00'),
(9, 1, 1, '2023-02-19 12:00:00', '2023-02-26 13:00:00'),

(14, 2, 1, '2023-02-18 12:00:00', null),
(15, 3, 1, '2023-02-19 12:00:00', null),
(16, 1, 1, '2023-02-19 12:00:00', '2023-02-20 13:00:00'),
(17, 3, 2, '2023-02-09 12:00:00', null),
(18, 4, 3, '2023-02-10 12:00:00', '2023-02-20 13:00:00'),
(19, 5, 4, '2023-02-11 12:00:00', '2023-02-12 11:00:00'),
(20, 6, 5, '2023-02-12 12:00:00', '2023-02-13 16:00:00'),
(21, 7, 1, '2023-02-19 12:00:00', '2023-02-20 09:00:00'),
(22, 8, 2, '2023-02-20 12:00:00', '2023-02-20 08:00:00'),

(23, 9, 1, '2023-02-19 12:00:00', '2023-02-20 13:00:00'),
(24, 9, 2, '2023-02-19 12:00:00', '2023-02-20 14:00:00'),
(25, 9, 3, '2023-02-19 12:00:00', '2023-02-21 15:00:00'),
(26, 10, 1, '2023-02-19 12:00:00', '2023-02-25 13:00:00'),

(27, 11, 1, null, '2023-02-25 01:00:00'),
(28, 11, 2, '2023-02-19 12:00:00', null),
(29, 11, 3, '2023-02-19 12:00:00', null),
(30, 12, 1, '2023-02-19 12:00:00', '2023-02-25 10:00:00'),
(31, 12, 2, '2023-02-19 12:00:00', '2023-02-25 10:00:00'),
(32, 12, 3, '2023-02-19 12:00:00', '2023-02-25 11:00:00'),
(33, 12, 4, '2023-02-19 12:00:00', '2023-02-25 13:00:00'),
(34, 12, 5, '2023-02-19 12:00:00', null),
(35, 13, 1, '2023-02-19 12:00:00', '2023-02-25 12:00:00');
```



## Code

### Create Tables

```sql
-- create Member table
CREATE TABLE Member (
  member_ID INT PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(50) NOT NULL,
  contact_number VARCHAR(20) NOT NULL
);

-- create GroupOrder table
CREATE TABLE GroupOrder (
  order_ID INT PRIMARY KEY,
  member_ID INT NOT NULL,
  FOREIGN KEY (member_ID) REFERENCES Member(member_ID)
);

-- create Package table
CREATE TABLE Package (
  package_ID INT PRIMARY KEY,
  owner_ID INT NOT NULL,
  order_ID INT NOT NULL,
  weight FLOAT NOT NULL,
  FOREIGN KEY (owner_ID) REFERENCES Member(member_ID),
  FOREIGN KEY (order_ID) REFERENCES GroupOrder(order_ID)
);

-- create HomeDeliveryOrder table
CREATE TABLE HomeDeliveryOrder (
  order_ID INT PRIMARY KEY,
  address VARCHAR(100) NOT NULL,
  FOREIGN KEY (order_ID) REFERENCES GroupOrder(order_ID)
);

-- create ServiceArea table
CREATE TABLE ServiceArea (
  service_area_ID INT PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  parent_area_ID INT,
  FOREIGN KEY (parent_area_ID) REFERENCES ServiceArea(service_area_ID)
);

-- create Locker table
CREATE TABLE Locker (
  locker_ID INT PRIMARY KEY,
  service_area_ID INT NOT NULL,
  name VARCHAR(50) NOT NULL,
  FOREIGN KEY (service_area_ID) REFERENCES ServiceArea(service_area_ID)
);

-- create LockerPickupOrder table
CREATE TABLE LockerPickupOrder (
  order_ID INT PRIMARY KEY,
  locker_ID INT NOT NULL,
  cell_number VARCHAR(10) NOT NULL,
  arrival_datetime DATETIME,
  collect_datetime DATETIME,
  FOREIGN KEY (order_ID) REFERENCES GroupOrder(order_ID),
  FOREIGN KEY (locker_ID) REFERENCES Locker(locker_ID)
);
```

### Q1

```php
<!DOCTYPE html>
<html>
<head>
	<title>assignment 2</title>
</head>
<body align=center>
	<h3>Q1 Answer</h3>
	<table border='1' align='center'>
		<thead>
			<tr>
				<th>member_ID</th>
				<th>name</th>
				<th>email</th>
				<th>contact_number</th>
			</tr>
		</thead>
		<tbody>
			<?php
			// Database connection
			include("connect.php");

			// SQL query
			$sql = "SELECT member_ID, name, email, contact_number FROM Member WHERE LOWER(name) LIKE '%tom%' ORDER BY member_ID DESC";
			$result = mysqli_query($conn, $sql);

			// Check if data exists
			if (mysqli_num_rows($result) > 0) {
			  // Output data of each row
			  while($row = mysqli_fetch_assoc($result)) {
			    echo "
			    <tr>
				    <td>" . $row["member_ID"] . "</td>
				    <td>" . $row["name"] . "</td> 
				    <td>" . $row["email"] . "</td> 
				    <td>" . $row["contact_number"] . "</td>
			    </tr>";
			  }
			} else {
			  echo "0 results";
			}

			mysqli_close($conn);
			?>
		</tbody>
	</table>
	<br><a href='index.html'>back</a>
</body>
</html>

```



From Q2~Q7, sql code only as the php format is exactly same as Q1:

### Q2 

```sql
SELECT HomeDeliveryOrder.order_ID, HomeDeliveryOrder.address, Member.member_ID, Member.name
FROM HomeDeliveryOrder
INNER JOIN GroupOrder ON HomeDeliveryOrder.order_ID = GroupOrder.order_ID
INNER JOIN Member ON GroupOrder.member_ID = Member.member_ID
WHERE LOWER(HomeDeliveryOrder.address) LIKE '%hong kong island%'
ORDER BY HomeDeliveryOrder.order_ID ASC;
```

### Q3

```sql
SELECT GroupOrder.member_ID, Member.name, COUNT(*) as service_count
FROM GroupOrder
INNER JOIN Member ON GroupOrder.member_ID = Member.member_ID
GROUP BY GroupOrder.member_ID
HAVING COUNT(*) > 3
ORDER BY service_count DESC, GroupOrder.member_ID ASC;
```

### Q4

```sql
SELECT Member.member_ID, Member.name, IFNULL(COUNT(LockerPickupOrder.order_ID), 0) as service_count
FROM Member
LEFT JOIN GroupOrder ON Member.member_ID = GroupOrder.member_ID
LEFT JOIN LockerPickupOrder ON GroupOrder.order_ID = LockerPickupOrder.order_ID
GROUP BY Member.member_ID
HAVING service_count < 3
ORDER BY service_count DESC, Member.member_ID ASC;
```

### Q5

```sql
SELECT GroupOrder.order_ID, SUM(Package.weight) as total_weight
FROM GroupOrder
INNER JOIN Package ON GroupOrder.order_ID = Package.order_ID
GROUP BY GroupOrder.order_ID
HAVING total_weight > 10
ORDER BY total_weight DESC, GroupOrder.order_ID ASC;
```

### Q6

```sql
SELECT LockerPickupOrder.order_ID, LockerPickupOrder.arrival_datetime, LockerPickupOrder.collect_datetime,
TIMESTAMPDIFF(HOUR, LockerPickupOrder.arrival_datetime, LockerPickupOrder.collect_datetime) - 48 as extra_hours
FROM LockerPickupOrder
WHERE LockerPickupOrder.collect_datetime > LockerPickupOrder.arrival_datetime + INTERVAL 2 DAY
ORDER BY extra_hours DESC, LockerPickupOrder.order_ID ASC;
```

### Q7

```sql
SELECT LPO.order_ID, L.name AS locker_name, SA.name AS service_area_name
FROM LockerPickupOrder LPO
JOIN Locker L ON LPO.locker_ID = L.locker_ID
JOIN ServiceArea SA ON L.service_area_ID = SA.service_area_ID
WHERE SA.name = 'Hong Kong Island' OR SA.parent_area_ID IN (
 SELECT service_area_ID FROM ServiceArea WHERE parent_area_ID = (
  SELECT service_area_ID FROM ServiceArea WHERE name = 'Hong Kong Island') OR parent_area_ID = ( SELECT service_area_ID FROM ServiceArea WHERE name = 'Hong Kong Island'))
```

### Q8

q8_submit.php:

```php
<!DOCTYPE html>
<html>

<head>
    <title>assignment 2</title>
</head>

<body align=center>
    <h3>Question 8_1</h3>
    <table border='1' align='center'>
        <tbody>
            <?php
            // Database connection
            include("connect.php");

            // SQL query
            $sql = "SELECT s.name AS service_area_name, COUNT(l.locker_ID) AS locker_count
            FROM ServiceArea s
            LEFT JOIN Locker l ON s.service_area_ID = l.service_area_ID
            GROUP BY s.service_area_ID
            HAVING locker_count >= 1
            ORDER BY locker_count DESC";
            $result = mysqli_query($conn, $sql);

            // Check if data exists
            if (mysqli_num_rows($result) > 0) {
                // Output data of each row
                while ($row = $result->fetch_assoc()) {
                    $options .= "<option value='" . $row['service_area_name'] . "'>" . $row['service_area_name'] . ": " . $row['locker_count'] . "</option>";
                }
            } else {
                echo "0 results";
            }

            mysqli_close($conn);
            ?>

            <!-- Display the drop-down menu and submit button -->
            <form action="q8.php" method="post">
                <select id="service_area" name="service_area_name">
                    <?php echo $options; ?>
                </select>
                <br><br>
                <input type="submit" value="Submit">
            </form>
        </tbody>
    </table>
</body>

</html>
```

Q8.php: 

```php
<!DOCTYPE html>
<html>

<head>
    <title>assignment 2</title>
</head>

<body align=center>
    <h3>Q8_2 Answer</h3>
    <table border='1' align='center'>
        <thead>
            <tr>
                <th>locker_ID</th>
                <th>locker_name</th>
                <th>uncollected_order_count</th>
            </tr>
        </thead>
        <tbody>
            <?php
            // Database connection
            include("connect.php");

            $selected_service_area_name = mysqli_real_escape_string($conn, $_POST["service_area_name"]);

            // SQL query
            $sql = "SELECT 
            l.locker_ID,
            l.name AS locker_name,
            (
              SELECT COUNT(*) 
              FROM LockerPickupOrder 
              WHERE locker_ID = l.locker_ID AND collect_datetime IS NULL
            ) AS uncollected_order_count
          FROM 
            Locker l 
            JOIN ServiceArea sa ON l.service_area_ID = sa.service_area_ID
          WHERE 
            sa.name = '$selected_service_area_name'
          ORDER BY 
            uncollected_order_count DESC;          
            ";
            $result = mysqli_query($conn, $sql);

            // Check if data exists
            if (mysqli_num_rows($result) > 0) {
                // Output data of each row
                while ($row = mysqli_fetch_assoc($result)) {
                    echo "
			    <tr>
				    <td>" . $row["locker_ID"] . "</td>
                    <td><a href='q9.php?locker_ID=" . $row["locker_ID"] . "'>" . $row["locker_name"] . "</a></td>
				    <td>" . $row["uncollected_order_count"] . "</td> 
			    </tr>";
                }
            } else {
                echo "0 results";
            }

            mysqli_close($conn);
            ?>
        </tbody>
    </table>
    <br><a href='index.html'>back</a>
</body>

</html>
```

### Q9

```php
<!DOCTYPE html>
<html>

<head>
    <title>assignment 2</title>
</head>

<body align=center>
    <h3>Q9 Answer</h3>
    <table border='1' align='center'>
        <thead>
            <tr>
                <th>locker_ID</th>
                <th>cell_number</th>
                <th>member_ID</th>
                <th>name</th>
                <th>contact_number</th>
                <th>package_count</th>
            </tr>
        </thead>
        <tbody>
            <?php
            // Database connection
            include("connect.php");

            // Get locker_ID from URL parameter
            $locker_ID = mysqli_real_escape_string($conn, $_GET["locker_ID"]);

            // SQL query
            $sql = "SELECT lp.locker_ID, lp.cell_number, m.member_ID, m.name, m.contact_number, COUNT(p.package_ID) AS package_count
            FROM LockerPickupOrder lp
            LEFT JOIN Package p ON lp.order_ID = p.order_ID
            LEFT JOIN GroupOrder go ON lp.order_ID = go.order_ID
            LEFT JOIN Member m ON go.member_ID = m.member_ID
            WHERE lp.locker_ID = '$locker_ID' AND lp.collect_datetime IS NULL
            GROUP BY lp.order_ID
            ORDER BY package_count DESC;            
    ";
            $result = mysqli_query($conn, $sql);

            // Check if data exists
            if (mysqli_num_rows($result) > 0) {
                while ($row = mysqli_fetch_assoc($result)) {
                    echo "
                    <tr>
                        <td>" . $row["locker_ID"] . "</td>
                        <td>" . $row["cell_number"] . "</td>
                        <td>" . $row["member_ID"] . "</td>
                        <td>" . $row["name"] . "</td>
                        <td>" . $row["contact_number"] . "</td>
                        <td>" . $row["package_count"] . "</td>
                    </tr>";
                }
            }
            mysqli_close($conn);
            ?>
        </tbody>
    </table>
    <br><a href='index.html'>back</a>
</body>

</html>
```

## Output

Q1:

| member_ID | name    | email            | contact_number  |
| --------- | ------- | ---------------- | --------------- |
| 2         | Tom Xie | tomxie@gmail.com | +1-555-555-5556 |

Q2:

| order_ID | address                                           | member_ID | name             |
| -------- | ------------------------------------------------- | --------- | ---------------- |
| 7        | CB320, KU, Central and Western, Hong Kong Island  | 7         | Oliver Nguyen    |
| 8        | WK321, KU, central and western, Hong Kong Island  | 9         | William Chen     |
| 11       | X293, KU, Central and Western, Hong Kong Island   | 12        | Charlotte Wilson |
| 12       | X920, KU, Central and Western, Hong Kong Island   | 13        | David Brown      |
| 13       | CH9283, KU, Central and Western, Hong Kong Island | 15        | Daniel Hernandez |

Q3:

| member_ID | name         | service_count |
| --------- | ------------ | ------------- |
| 6         | Isabella Lee | 8             |
| 21        | Aaliyah Lee  | 5             |

Q4:

| member_ID | name             | service_count |
| --------- | ---------------- | ------------- |
| 1         | John Doe         | 1             |
| 2         | Tom Xie          | 1             |
| 3         | John Smith       | 1             |
| 4         | Ava Rodriguez    | 1             |
| 5         | Ethan Davis      | 1             |
| 8         | Sophia Kim       | 1             |
| 9         | William Chen     | 1             |
| 10        | Mia Thompson     | 1             |
| 11        | Michael Martinez | 1             |
| 12        | Charlotte Wilson | 1             |
| 13        | David Brown      | 1             |
| 14        | Harper Lee       | 1             |
| 15        | Daniel Hernandez | 1             |
| 16        | Avery Collins    | 1             |
| 17        | Natalie Baker    | 1             |
| 18        | Alexander Chen   | 1             |
| 7         | Oliver Nguyen    | 0             |
| 19        | Evelyn Kim       | 0             |
| 20        | Liam Clark       | 0             |
| 22        | Lucas Jones      | 0             |
| 23        | Madison Lee      | 0             |
| 24        | Jacob Hernandez  | 0             |
| 25        | Aurora Wilson    | 0             |
| 26        | Benjamin Kim     | 0             |
| 27        | Violet Brown     | 0             |
| 28        | Avery Martinez   | 0             |
| 29        | Kelly Yuen       | 0             |

Q5:

| order_ID | total_weight |
| -------- | ------------ |
| 20       | 103          |
| 19       | 101          |
| 21       | 91           |
| 18       | 86           |
| 1        | 56           |
| 25       | 36           |
| 33       | 32           |
| 34       | 31           |
| 32       | 28           |
| 5        | 24           |
| 6        | 24           |
| 4        | 23           |
| 31       | 23           |
| 17       | 22           |
| 2        | 21           |
| 3        | 21           |
| 23       | 21           |
| 22       | 20           |
| 30       | 20           |
| 35       | 20           |
| 27       | 19           |
| 9        | 18           |
| 28       | 17           |
| 26       | 15           |

Q6:

| order_ID | arrival_datetime    | collect_datetime    | extra_hours |
| -------- | ------------------- | ------------------- | ----------- |
| 18       | 2023-02-10 12:00:00 | 2023-02-20 13:00:00 | 193         |
| 9        | 2023-02-19 12:00:00 | 2023-02-26 13:00:00 | 121         |
| 2        | 2023-02-15 12:00:00 | 2023-02-21 13:00:00 | 97          |
| 26       | 2023-02-19 12:00:00 | 2023-02-25 13:00:00 | 97          |
| 33       | 2023-02-19 12:00:00 | 2023-02-25 13:00:00 | 97          |
| 35       | 2023-02-19 12:00:00 | 2023-02-25 12:00:00 | 96          |
| 32       | 2023-02-19 12:00:00 | 2023-02-25 11:00:00 | 95          |
| 30       | 2023-02-19 12:00:00 | 2023-02-25 10:00:00 | 94          |
| 31       | 2023-02-19 12:00:00 | 2023-02-25 10:00:00 | 94          |
| 6        | 2023-02-19 12:00:00 | 2023-02-23 13:00:00 | 49          |
| 3        | 2023-02-19 12:00:00 | 2023-02-22 13:00:00 | 25          |
| 25       | 2023-02-19 12:00:00 | 2023-02-21 15:00:00 | 3           |

Q7:

| order_ID | locker_name                     | service_area_name |
| -------- | ------------------------------- | ----------------- |
| 1        | HKU Station Locker              | Pokfulam          |
| 4        | HKU Station Locker              | Pokfulam          |
| 9        | HKU Station Locker              | Pokfulam          |
| 16       | HKU Station Locker              | Pokfulam          |
| 2        | Pofulam Convenient Store Locker | Pokfulam          |
| 5        | Pofulam Convenient Store Locker | Pokfulam          |
| 14       | Pofulam Convenient Store Locker | Pokfulam          |
| 3        | Pofulam Gas Station Locker      | Pokfulam          |
| 6        | Pofulam Gas Station Locker      | Pokfulam          |
| 15       | Pofulam Gas Station Locker      | Pokfulam          |
| 17       | Pofulam Gas Station Locker      | Pokfulam          |

Q8:

Q8_submit.php:

![q8_submit]({{site.url}}/images/2023-03-17-BasicSQLPractice/q8_submit.png)


Q8.php(user selects Clear Water Bay: 9 for example):

| locker_ID | locker_name    | uncollected_order_count |
| --------- | -------------- | ----------------------- |
| 11        | HKK Locker A   | 2                       |
| 12        | HKK Locker B   | 1                       |
| 6         | HKSTP Locker C | 0                       |
| 13        | HKK Locker C   | 0                       |
| 10        | HKUST Locker B | 0                       |
| 14        | HKK Locker D   | 0                       |
| 4         | HKSTP Locker A | 0                       |
| 15        | HKK Locker E   | 0                       |
| 5         | HKSTP Locker B | 0                       |

Q9 (user selects HKK Locker A for example):

| locker_ID | cell_number | member_ID | name         | contact_number  | package_count |
| --------- | ----------- | --------- | ------------ | --------------- | ------------- |
| 11        | 2           | 21        | Aaliyah Lee  | +1-555-555-5572 | 2             |
| 11        | 3           | 6         | Isabella Lee | +1-555-555-5557 | 0             |
