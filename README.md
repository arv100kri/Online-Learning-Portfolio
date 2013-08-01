Here are the new requirements for Tachyon:
==========================================

1. The call to "GET All Contacts", should return only the following fields for each contact
  a) Contact's ID
	b) Contact's Primary Contact Information (as mentioned previously)
	c) If there are multiple emails associated with that contact, then the "Primary" email as deemed by Yahoo! Mail.
		[*: Else, among the multiple emails, add a field {primary: true}]
	d) If there are multiple phones associated with that contact, then the "Primary" phone as displayed in a contact's info on Yahoo! Mail.
		[*: Else, among the multiple phones, add a field {primary: true}]
	e) The name of the person in the following format
		fieldName: 'name',
		value: {
			firstName: 'Abcd',
			lastName: 'Xyz'
		}
		There is no need for any other names, like middleName, prefix, suffix etc., for now
		[*: Else, its ok if you return all of them, but please make do let me know what the keys are. For instance, the API has it as givenName, familyName etc.,]
	f) The address is not needed in the call to GET All Contacts, but if you have it already, thats fine.
	g) The categories to which the contact belongs to in the following format
		categories: [
		{category: 'Friends'},
		{category: 'Family'}]
	h) The Yahoo! Messenger ID if available/present
There might be one or more of the above missing.
I still need the requirement of sorting the contacts into alphabetical buckets based on the primary contact information, which I specified earlier.
A complete example is in the file "desired_response.json"


2. The call to "GET Contact", should return response similar to the following
{
	id: "1",
    primaryContactInformation: 'Jack Sparrow',
    fields: [
        {
            fieldName: "name",
            value: {
                firstName: "Jack",
                lastName: "Sparrow"
            }
        },
        {
            fieldName: "email",
            value: "jack@sparrow.com",
        },
        {
        	fieldName: "email",
        	value: "sample@domain.com",
        	FLAGS: "WORK"
        },
        {
            fieldName: "phone",
            value: "404-436-7334",
        },
        {
            fieldName: "yahooid",
            value: "example_id"
        },
        {
        	fieldName: "link",
        	value: "www.example.com"
        },
        {
            fieldName: "address",
            value: {
                street: "Address Line 1\nAddress Line 2",
                city: null,
                stateOrProvince: null,
                postalCode: null,
                country: "United States",
                countryCode: "US"
            },
            flags: "HOME"                 
        }
    ],
    categories: [
        {category: 'Family'},
        {category: 'Band Members'}
    ]
}

3. Get all categories of the user --> There is no change here. I just expect the response to be in the following format:
	categories: [
	{category: 'Family'},
	{category: 'Friends'},
	...
	]

4. Get all contacts in a category --> There is no change here. My app will provide a category name to tachyon. The response should be in the same format as for (1) i.e., with
the sorting and alphabetical buckets.

5. Get all categories to which a contact belongs to --> This is a new requirement, due to a change in the design/functionality. I will pass a contact id, and expect the response
to be in the same format as (3)

6. To delete a contact, my app will simply pass the contact id and the guid of the user. No change there. Please give back the same HTTP response which the API gives

7. Add a new list/category: --> Another new requirement. The request will be in the format
	{category: 'New Category'}

8. Add a new contact.
	a) My app will construct the contact object as follows:
	{
		fields: [
			{
				fieldName: 'name',
				value: {
					firstName: 'xyz',
					lastName: 'abc'
				}
			},
			{
				fieldName: 'phone',
				value: '1232312312',
				FLAGS: 'HOME'
			},
			....
		],

		categories: [
			{category: 'Family'},
			{category: 'India'},
			...
		]
	}
	This needs to be inserted to the user's addressbook

	b) In case one or more of the categories are not there, then create that category before inserting into the addressbook. If tachyon can do this it will be great,
	else I can handle it in my code by making appropriate calls.

	c) I do some validations on my side both in the client as well as in the node server. I don't know if you will be doing appropriate validations as well.

9. Search contacts --> New Requirement
	a) Request will be {searchTerm: "search"}
	b) Response need not be sorted/bucketed according to alphabetical order. Instead simply return an array of contact objects, i.e.,
	{
		count: "7",
		contacts: [
			{
				id: "1",
			    primaryContactInformation: 'Jack Sparrow',
			    fields: [
			        {
			            fieldName: "name",
			            value: {
			                firstName: "Jack",
			                lastName: "Sparrow"
			            }
			        },
			        {
			            fieldName: "email",
			            value: "jack@sparrow.com",
			        },
			        {
			        	fieldName: "email",
			        	value: "sample@domain.com",
			        	FLAGS: "WORK"
			        },
			        {
			            fieldName: "phone",
			            value: "404-436-7334",
			        },
			        {
			            fieldName: "yahooid",
			            value: "example_id"
			        },
			        {
			        	fieldName: "link",
			        	value: "www.example.com"
			        },
			        {
			            fieldName: "address",
			            value: {
			                street: "Address Line 1\nAddress Line 2",
			                city: null,
			                stateOrProvince: null,
			                postalCode: null,
			                country: "United States",
			                countryCode: "US"
			            },
			            flags: "HOME"                 
			        }
			    ],
			    categories: [
			        {category: 'Family'},
			        {category: 'Band Members'}
			    ]
			},
			{
				...
			},
			{
				...
			}
		]
	}
	c) Count is the number of contacts matched. It is not very essential, but would be nice if I got this as well

10. Requirements such as (a) Remove contact from category, (b) Editing a contact (and other associated to edit) are no longer needed.
