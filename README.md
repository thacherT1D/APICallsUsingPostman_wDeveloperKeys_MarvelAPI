##Learning API GET Calls with Postman and the Marvel API

When using APIs sometimes you will need to obtain/use developer keys. When using these in an application you will usually have to include the public key and a hashed version of the private key plus some other variable(s).

Rather than using random webpages to find a hash tool, you can use node right in your terminal. And then test your GET calls from Postman.

###Getting Started with an API
A pretty safe bet to find the information for any API is to just google the company name followed by "API" or "Developer Portal". In this case we're going to use the Marvel API as an example.

After finding the [Marvel Developer Portal](http://developer.marvel.com/) and creating an account, I'm able to access my public and private developer keys.

Further reading the docs I'm able to see what is required to make a GET call to the API.

Per the Marvel API docs, the formula to construct a valid call is as follows:
* A user with a public key of "1234" and a private key of "abcd" could construct a valid call as follows: `http://gateway.marvel.com/v1/comics?ts=1&apikey=1234&hash=ffd275c5130566a2916217b101f26150`
* The hash value is the md5 digest of 1abcd1234 OR (ts)(private key)(public key)

**Optional Note (if you're interested) Quick note on developer keys and hashing:** Hashing, or A cryptographic hash function, is a mathematical algorithm that maps data of arbitrary size to a bit string of a fixed size (a hash function) which is designed to also be one-way function, that is, a function which is infeasible to invert. The only way to recreate the input data from an ideal cryptographic hash function's output is to try a large number of possible inputs to see if they produce a match. [wikipedia](https://en.wikipedia.org/wiki/Cryptographic_hash_function)

There is a lot more to this, but this is all you need to know for this exercise is that you're going to use your public key, private key, and a timestamp to create a hashkey to be used in your API call to the Marvel API. 

###Two ways to create your hash
Below I've outlined two ways to create your hash. If you have Node.js installed on your computer and feel comfortable using the terminal, using Node to create your hash is the best option. Likewise, if you don't know if you have node installed, or don't feel as comfortable with the command line, we can also create a hash using a web tool.

####Using a web tool to create your hash

####Using Node to create your hash
Using your developer keys and a chosen timestamp of '123456789', you can now create your hash to be used with the API call.

From your terminal type `node` -- you should then see `>` on the left hand side of the terminal. Below is what the input and output will look like for node.

**Set the Variables**

`> var private = 'YOUR PRIVATE KEY'`

`undefined`

`> var public = 'YOUR PUBLIC KEY'`

`undefined`

`> var ts = '123456789'`

`undefined`

**Create the hash**

`crypto.createHash('md5').update(`${ts}${private}${public}`).digest('hex');`

`'YOUR HASH'`

**Type `.exit` to exit node.**

Now that you have all the pieces, we can make a test call using Postman.

###Getting Started with Postman
**[Postman Docs](https://www.getpostman.com/)**

You can use the web-based or desktop version of Postman.

###Testing GET requests with Postman
From the Marvel API documentation we know that our GET request needs to be in the following format:
* A user with a public key of "1234" and a private key of "abcd" could construct a valid call as follows: `http://gateway.marvel.com/v1/comics?ts=1&apikey=1234&hash=ffd275c5130566a2916217b101f26150`

We can build that in Postman by first selecting "GET", putting  the beginning of the url `http://gateway.marvel.com/v1/public/characters/1011396`
in the text box and then pressing the "Params" button. For each of the parameters required by the Marvel API, add a param key and value. Then click "Send" -- and the results of your call (whether information or error) will be shown in the window below. You can customize how you view the data in Postman using the options listed above the text window with the call results.
