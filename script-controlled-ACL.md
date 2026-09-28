Script-Controlled ACL – Restrict Record Access Based on Field Value
Description :
This lab demonstrates how to implement a script-controlled Access Control List (ACL) to restrict users from viewing records unless they meet specific conditions. In this scenario, only users belonging to the EEE branch are allowed to view records where the Branch field is set to EEE, while administrators retain full access.
Timer :
3 hrs
Milestone-1 Creation of Users and Roles
User Creation:
Log in to the ServiceNow instance with admin access.
Create a test user:
Navigate to User Administration → Users
Click New
User ID: EEE User
First name: EEE
Last name: User
Email: eeeuser@gmail.com
Save the record


Role Creation
Navigate to User Administration → Roles
Click New
Name: Enter role name as bb1
Description: Brief role purpose
Click Submit
Create other roles also like bb2,bb3,bb4.(follow the same procedure)
Assign all the roles to the EEE User


Tables Creation
In the Application Navigator, search for System Definition → Tables.
Click New and create a custom table with the following details:
Label: Institution Details
Name: u_institution_details
Extends for : false




Click Submit.
Add the following fields to the u_institution_details table:
Student Roll Number– Auto Number
Student Name – Reference- User
Faculty Name – Reference- User
Branch – Choice (ECE, EEE, CSE)
Email– String
Phone Number– String
Description– Multi String
Save the Table

Create multiple records in the Student Records table with different branch values (ECE, EEE, CSE).

Creation of Access Control Lists(Read ACL)
In the Application Navigator, search for System Security → Access Control (ACL).
Elevate “Security_admin” role
Click New to create a new ACL.
Enter the following details:
Type: record
Operation: read
Name: u_institution_details
Active: true
Advanced: true
In the Requires role related list, add:
bb1 (custom role)
Add Data condition as Branch is EEE
In the Script field, add the following script:
(function () {
    // Allow admin users full access
    if (gs.hasRole('admin')) {
        return true;
    }
    // Allow only EEE branch users to see EEE records
    if (gs.hasRole('bb1')){
        return true;
    }
    // Deny access for all others
    return false;
})();
Click Submit to save the ACL.






Impersonate EEE users and open the Student Records list.
Copy the table name u_institution_details.list/LIST and paste in the All menu.
Impersonate Admin  and open the Student Records list.
Verify :
User with bb1 role can view only EEE branch records

User without the role cannot view any records

Admin user can view all records regardless of branch

Creation of Access Control Lists(Create ACL)
Click New to create a new ACL(Create)
Enter the following details:
Type: record
Operation: create
Name: u_institution_details
Active: true
In the Requires role related list, add:
bb2 (custom role)
No need to add any Data condition
Save the ACL Record 




Verify :
User with bb1 &bb2 roles can view only EEE branch records b New Button

Creation of Access Control Lists(Write ACL)
Click New to create a new ACL(Write)
Enter the following details:
Type: record
Operation: Write
Name: u_institution_details
Active: true
In the Requires role related list, add:
bb3 (custom role)
No need to add any Data condition
Save the ACL Record 




Verify :
User with bb1 &bb2 &bb3 roles can view and edit the  EEE branch records along with New Button



Creation of Access Control Lists(Delete ACL)
Click New to create a new ACL(Delete)
Enter the following details:
Type: record
Operation: Delete
Name: u_institution_details
Active: true
In the Requires role related list, add:
bb4 (custom role)
No need to add any Data condition
Save the ACL Record 




Verify :
Users with bb1 &bb2 &bb3&bb4 roles can see, create and edit along with delete the  EEE branch records.





Outcome :
By completing this lab, learners gain a comprehensive understanding of how script-controlled READ, WRITE, CREATE, and DELETE ACLs enforce record-level security, how user roles and record field values are evaluated together to make access decisions, and how ACLs protect sensitive data from unauthorized viewing, modification, creation, and deletion across forms, lists, and Service Portal interfaces.


