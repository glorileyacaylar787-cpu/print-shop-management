# print-shop-management

Print Shop Management System

An app I built in AppSheet (using Google Sheets as the database) for a custom printing and graphic design shop. This README is a simple overview of everything the app does and how it was built, so it can also serve as my guide for the midterm interview.

1. What This App Is For

This app is for a print shop that takes orders from customers and buys materials from suppliers. Before, this kind of information would just be written down or kept in separate sheets. This app puts everything in one system: customers, their orders, the products being sold, and the materials bought from suppliers to make those products.

2. Who Uses It

Right now there is one user role: the shop staff/admin. They are the one who adds customers, records orders, logs purchases, and checks the dashboard. Customers and suppliers are not separate users who log in — they're just the people and businesses the shop deals with, so their information is stored as data in the system.

3. The 9 Tables (Entities)

The database has 9 tables, connected to each other:

Customer — the people who order from the shop.
Orders — each order a customer makes.
OrderDetails — the actual items inside one order (which product, how many, how much).
Products — the things the shop sells (t-shirts, mugs, tarpaulins, etc.).
Category — groups the products (Apparel, Signage, Merch, etc.).
Supplier — the businesses the shop buys materials from.
Purchases — each purchase made from a supplier.
PurchaseDetails — the actual materials inside one purchase (which material, how many, how much).
Materials — the raw materials used to make products (vinyl, ink, paper, etc.).

How they connect:

One Customer can have many Orders.
One Order can have many Products in it, and one Product can be in many Orders — this is handled by OrderDetails, which lists one product per row for every order.
One Category can have many Products.
One Supplier can have many Purchases.
One Purchase can have many Materials in it, and one Material can be bought in many Purchases — this is handled by PurchaseDetails, the same way OrderDetails works.
4. What the App Can Do
Add, view, edit, and delete records for every table (Customers, Orders, Products, Suppliers, Materials, Purchases, and their detail tables).
Add items while creating an order or purchase. When adding a new Order, you can add its products right away in the same screen — no need to save first then come back. The same was set up for Purchases and its materials.
Dashboard that shows totals and counts, not just plain lists. It shows:
Which products are earning the most (total revenue per product).
Which customers still have pending orders.
How much of each material has been purchased.
A simple summary view that shows order details together with the customer and product info, so you don't have to click through multiple tables just to read one order.
A filtered view that shows only pending orders, so staff can quickly see what still needs to be done.
Auto-generated IDs. When adding a new customer or purchase, the system automatically creates the ID — the user doesn't type it in themselves.
Easy-to-read receipt numbers. Instead of showing the raw database ID (like a long code), the app shows a simple receipt number such as OR-2026-00007 for orders and PU-2026-00012 for purchases.
Input checking. For example, contact numbers must start with "09" and have exactly 11 numbers, and name fields can't contain numbers. If someone enters something wrong, the app shows a message explaining what's wrong.
5. A Few Decisions I Made and Why
OrderDetails uses two IDs together as its key (OrderID + ProductID) instead of a separate single ID, because that combination is already unique on its own — no single order-and-product line needs to be looked up on its own elsewhere.
PurchaseDetails uses its own separate ID (PurchaseDetailID) instead, to match how the original data was given and so each purchase line can be tracked individually.
The summary view does not change how data is stored. The actual tables stay clean and organized (normalized); the summary view just displays the same data in a more readable way, joined together. Nothing is duplicated in storage.
The dashboard only shows totals and counts, never a plain list. A list of raw records is not considered a proper dashboard, so every panel adds up or counts something instead of just displaying rows.
The raw ID numbers are hidden from the screen. Staff don't need to see or understand the technical IDs — they just need the receipt number, so those are shown instead while the real ID still works in the background.
6. Dashboard Panels (In Detail)
Panel	What it shows	What it's made from
Revenue by Product	Total money earned per product	Adds up all the Subtotals from OrderDetails for each product
Pending Orders by Customer	How many pending orders each customer has	Counts Orders where Status = Pending, grouped by customer
Material Stock Activity	Total quantity bought per material	Adds up all the QuantityBought from PurchaseDetails for each material
7. Tools Used
AppSheet — the no-code tool used to build the app itself (screens, buttons, logic).
Google Sheets — where all the data actually lives, behind the scenes.
8. How the App Was Set Up (Step by Step)
Put all the data into a Google Sheet, one tab per table.
Create a new app in AppSheet using that Google Sheet as the source.
For each table, set which column is the unique ID (Key) and which column should display as the label (like the name).
Connect related tables using Reference columns (this is how foreign keys work in AppSheet), and turn on "Is a part of?" for detail tables so their items can be added inline.
Add the computed columns (totals, counts), the filtered view, and the Dashboard.
Test everything — adding, editing, deleting records, and checking the dashboard — before considering it done.
9. Problems I Ran Into and What I Learned
AppSheet doesn't have some functions other programs have, like checking text patterns (no REGEXMATCH) or checking if something is a number (no ISNUMBER). I had to build the same checks using simpler functions like LEFT, RIGHT, LEN, and CONTAINS.
I once accidentally made a column reference its own table, which broke the whole app. I learned to always double check what a Reference column is pointing to.
There's a difference between "Initial Value" and "App formula" in AppSheet. Initial Value only runs once, when the record is first created — that's the right choice for auto-generating an ID. App formula keeps recalculating every time, which would actually break an ID if used wrong.
To let someone add detail items (like order items) while still creating the main record, the detail table's reference column needs "Is a part of?" turned on. Without it, you can only add those items after saving first.
A dashboard is graded on whether it totals or counts something. A plain list, even if useful, does not count as a real dashboard panel.

Made by Gloriley Dagpin Acaylar — CpE 118 Database Management
