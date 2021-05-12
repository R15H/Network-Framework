# IRC-Network-framework



# Protocol


Is implemented in Message.py.   

Example of packet from the server:

```
**Status:** SUCCESS   
**Data:** positionalArg1 positonalArg2...  NAME#Flávio ID#3 POINTS#200  @ NAME#Júlio POINTS#600 ID#8   
**Message:** This is an human readable message.   
It can have multiple lines with arbitrary content like so:  
Data: this is still part of the message  
```

Data is parsed in Python by to:

```python
ServerMessage.positionalArgs = tuple[str]
ServerMessage.keywordArgs = list[ dict[str,str] ]
```
The *@* token is used to separate each dict placed in keywordArgs.

So given the example data would be:

```python
ServerMessage.positionalArgs = (positionalArg1, positionalArg2)
ServerMessage.keywordArgs = [ {'NAME' : "Flávio", "ID": "3", "POINTS" : "200" }, { "NAME" : "Júlio", "POINTS" : "600", "ID" :"8"}    ]
```


Example of packet from the client:

```
( the same syntax as the data field from the server but does not support lists of dicts )
```
Implemented in the ClientMessage.


**Obs:** The notation used is defined in the pydocs [here](https://docs.python.org/3/library/typing.html)



# User login and Registrarion

Done through the following command:

```
IAM **name** **microservice** **pass**
```

This command triggers a new control flow on both the server and the client.   
The following steps occur for a successful user login:

1. Command is sent to the server
2. IF the service does not provide the requested service it either returns the IP of someone who does or warns the user that this is an unknown service


3. Looks up for the provided name in the list of users 
   . If found tries to match the given password to the stored user password  
5. Good password :heavy_check_mark:

-1 Client code detects the microservice specified and sees if it has the IP address of the requested service

 
 
### Custom modules conventions:

module.commandPallet  = {  'commandName' , commandFunctionObject }  
commandFuntionObject( message ) and raises InvalidMessageContents

# User

An user is __identified__ by it's IP address, has a name and a set of *roles* where each type confers permissions to execute commands.  
Every user is of role "anonymous" by default.



The name and types of a user can be modified using the command *IAM* -/+ROLE NAME.
 + adds the role to the user
 - removes the role from the user 
Name and type are mandatory arguments even if one does not itend to modify the type or the name.

Internaly restricting and expanding user permissions is done by calling user.expandPermissions(role) or user.restrictPermissions(role)


# Storage


Storage loads and builds tables from files according to a schema present in the first two lines of every loadable file. 

## API
 ### Transactions
 
 storage.beginTransaction(['table1', 'table2', 'table3'])   --> locks this tables
 storage.commitTransaction()
 
 -------
 Notes:   
 Only one transaction can happen at a given time for each storage object.
 
 ### Data operations
 storage.updateItem(id, table, dataDict)
 storage.massiveUpdate(tables, where, how)
 
 storage.getItem(wantedKeys,    id, table)  
 storage.massiveGet(wantedKeys, tables, where)
    
 storage.insertItem(table, itemDict)
 
 storage.executeQuery(str)
