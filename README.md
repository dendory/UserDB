### NAME

UserDB - A simple users and groups management interface

### SYNOPSIS

    use UserDB;
      
    my $userdb = UserDB->new("user.db");   # Connect to the database
    if(!$userdb) { die "Could not connect to database!"; }
    
    $userdb->create_user("tanya");   # Create a new user
    $userdb->set_attributes("tanya", name => "Tanya Harding", email => "tanya.harding\@example.com");   # Set some attributes
    my %attrs = $userdb->get_attributes("tanya");   # Get attributes
    
    print "Name: " . $attrs{"name"} . "\n";
    print "Email: " . $attrs{"email"} . "\n";
    print "Department: " . $attrs{"department"} . "\n";
  
    $userdb->create_group("Finance Staff");   # Create a group
    $userdb->add_to_group("tanya", "Finance Staff");   # Add user to group
  
    $userdb->create_user("joe");   # Create a new user
    $userdb->add_to_group("joe", "Finance Staff");   # Add the new user to group
    
    foreach my $member ($userdb->members_of_group("Finance Staff"))   # List members of group
    {
        print "Finance Staff contains: " . $member . "\n";
        $userdb->remove_from_group($member, "Finance Staff");    # Remove member from the group
    }
    
    $userdb->set_password("joe", "Test123");   # Set a password
    if($userdb->check_password("joe", "Test321"))   # Verify the password
    {
        print "Login successful!\n"; 
    }
    else 
    {
        print "Login failed! " . $userdb->error . "\n"; 
    }

### DESCRIPTION

UserDB is a simple management module for users and groups. It uses a flat file database to store information and as such does not rely on any external resource. It provides an interface to do simple functions for implementing users and groups, and handles errors gracefully. Any method can return 'undef' in case an error happened, the expected value (or true) otherwise.

### METHODS

$userdb = UserDB->new($filename)
Create or open an existing UserDB file.

$userdb->error
Returns the last error message.

$userdb->create_user($username)
Create a new user.

$userdb->set_attributes($username, %attributes)
Set attributes for a user. The available attributes include: name, email, profile, phone, manager, department, url, notes.

%attributes = $userdb->get_attributes($username)
Returns an associative array of attributes for the user.

$userdb->set_password($username, $password)
Set a user password. The password is hashed before being stored.

$userdb->check_password($username, $password)
Check a user password against the stored one.

@users = $userdb->list_users
Return a full list of users.

$userdb->create_group($groupname)
Create a new group.

$userdb->add_to_group($username, $groupname)
Add a user to a group.

$userdb->remove_from_group($username, $groupname)
Remove a user from a group.

@members = $userdb->members_of_group($groupname)
Returns an array of members for a group.

### DEPENDENCIES

DBI
Digest::SHA

### AUTHOR

Patrick Lambert, <dendory@live.ca>
COPYRIGHT AND LICENSE ^

Copyright (C) 2014 by Patrick Lambert

This library is free software; you can redistribute it and/or modify it under the same terms as Perl itself, either Perl version 5.16.3 or, at your option, any later version of Perl 5 you may have available.
