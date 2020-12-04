# Microservice Notes
* One simple microservice is a stream of random numbers
1. Create a stream object
2. Create a really long byte string of secure random data (1024 bytes)
3. Read the next chuck of bytes out, depending on the size based on the rest endpoint
4. BigInteger for a Long or Int.
5. BigDecimal for Double or Float (A BigInteger, and an Int from a BigInteger for scale)
