#Address Class

It represents an address of an account in the ICON Network.

```java
java.lang.Object score.Address
public class Address extends java.lang.Object
```

## Field

The length of an address. The length has value of 21.

```java
public static final int LENGTH
```

## Constructor

Creates an address with the contents of the given raw byte array.
```java
public Address(byte[] raw) throws java.lang.IllegalArgumentException
```

    Parameters: raw - a byte array
    Throws:     java.lang.NullPointerException - if the input byte array is null 
                java.lang.IllegalArgumentException - if the input byte array length is 
                invalid

## Methods:
	
**fromString**

Create an address from the hex string format
```java
public static Address fromString(java.lang.String str)
```

	Parameters:	 str - a hex string that represents an Address
	Returns:	 the resulting address
	Throws: 	 java.lang.NullPointerException - if the input string is null
			 java.lang.IllegalArgumentException - if the input string format or 
			 length is invalid


**isContract**

Returns true if and only if this address represents a contract address, false otherwise

```java
public boolean isContract()
```


**toByteArray**

Converts this address to a new byte array.
```java
public byte[] toByteArray()
```

	Returns:    a newly allocated byte array that represents this address


**hashCode**

Returns a hash code for this address.

```java
public  int hashCode()
```

    Overrides:  hashCode in class java.lang.Object
    Returns:    a hash code value for this object


**Equals**

Compare this address to the specified object.
```java
public boolean equals(java.lang.Object.obj)
```

    Overrides:  equals in class java.lang.Object
    Parameters: obj - the object to compare this address against
    Returns:    true if the given object represents an Address equivalent to this address, 
                false otherwise

**toString**

Returns a string representation of this address.

```java
public java.lang.String.toString()
```

    Overrides:  toString in class java.lang.Object
    Returns:    a string representation of this object

