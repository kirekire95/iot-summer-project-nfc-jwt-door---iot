## Authentication system featuring a prototype door, NFC technology and JSON Web Tokens

#### Erik Claesson - ec222qs

In this project I am going to talk about how I built my own authentication system featuring a door that can be opened with an NFC card along with the help of a JSON Web Token (JWT) stored onto it for extra security.

This project also briefly goes over my technologies of choice, such as Next.js, GraphQL, Prisma and Redis, etc and how I used these technologies together in a microservice architecture.

I will provide a brief overview but without going into too much detail here about my choice of technologies and what they are:

1. **Next.js**, the React Framework, is a solution to build hybrid applications built with static assets and serverless functions. It is essentially a fullstack framework that gives you a backend and a frontend to work with.
2. **GraphQL** is a query language for your API, and a server-side runtime for executing queries using a type system that you can define for your data, and this is arguably a better approach compared to something more traditional like REST.
3. **Prisma** is a Next-generation Node.js and TypeScript ORM which I use Postgres with. With Prisma, you define your models in the declarative Prisma schema which serves as the single source of truth for your database schema and the models in your programming language of choice.
4. **Redis** is an in-memory data structure store which I use as a message broker through the publish-subscribe-pattern, as well as caching GraphQL queries with persisted queries, and storing JSON Web Tokens.

Estimated time: 60+ hours - depends greatly on knowledge and familiarity.
Estimated price: 3000 SEK

### Objective

My goal when building this project was to do something that involved interacting with a door from a NFC card, and even better if JSON Web Tokens could be involved with this I figured. The idea of combining software written on the computer with electronics in real life was incredible to me, and so it has been quite the journey all the way from start to finish with this project.

In the end, the purpose and insights of this project has mainly been educational and for myself to see what code and electronics can achieve together when combined. There is however also a valid use case for this as I definitely can see the need for more smart authentication systems in the near future.

### Material

The material used for this project includes:

TODO: Finish this up
| Material | Description | Cost (SEK) | Store |
|----------|-------------|------------|-------|
| | | | |
| | | | |
| | | | |

1. **Raspberry Pi 4** - In my project I decided to use a Raspberry Pi 4 to spin up a Node.js server, and by leveraging a couple of NPM libraries I could then communicate with any lights, buzzers, the solenoid, the PN532, and so on.
2. **Breadboard** - At least one breadboard should be required to wire up all the electronics. In my case I used three different breadboards for better separation of concerns.
3. **Breadboarding Female/Female Jumper Wires**
4. **Breadboarding Female/Male Jumper Wires**
5. **Breadboarding Male/Male Jumper Wires** - I used the jumper cables to connect the GPIO pins to the breadboard. I also used jumpers from the breadboard to the solenoid and to the DC-jack.
6. **TIP120 NPN Power Darlington Transistor** - The transistor is used to drive various devices that have high power requirements, such as the 12V solenoid. This is necessary since I do not want to directly interact with the solenoid through my Raspberry Pi.
7. **Adapter DC-jack** - The DC-jack is used to power up the 12V solenoid with the help of a 12V DC Power Adapter.
   One could also use batteries or a powerbank for this which would provide better mobility if needed.
8. **1N4007 DO-41 1000V 1A diode** - The diode is used to allow flow of current only in one direction which is all I needed.
   at makes a beeping noise. I use this for the cases when the JWT has been written to the NFC card, or when the door has been interacted with.
9. **Solenoid 12V** - The solenoid is essentially an electronic lock which in my case is used as the lock mechanism for the door. When 9-12VDC is applied, the slug pulls in so it doesn't stick out anymore and the door can then be opened. Draws 650mA at 12V, 500 mA at 9V when activated.
10. **12V DC Power Adapter** - I had a generic 12V DC Power Adapter laying around, and so I used this to power the DC-jack which in turn would give power to the solenoid.
11. **FTDI-cable USB/TTL** - The FTDI-cable is used to convert TTL serial transmissions to and from USB signals which in my case results in data being sent to the Raspberry Pi from the PN532 device.
12. **PN532 NFC/RFID** - The PN532 is a popular device which allows reading and writing to for example an NFC card which in turn will interact with the door. The device can be used via I2C, UART or SPI communication protocols. In my case I used the UART protocol for simplicity.
13. **NTAG203 13.56 MHz RFID/NFC-cards** - These NFC cards are often used as train or bus passes. The tag contains a small RFID chip and an antenna, and is passively powered by the reader/writer when placed a couple inches away.
    A couple of things to note here is that these chips can be written to and store only up to **144 bytes** of data in writable EEPROM divided into 4 byte banks divided in 36 pages. This is not enough for a large size JWT, but it is enough for many use cases nevertheless. Another thing to note is that unlike "Classic 1K" cards, these tags are more secure and work with almost any phone with RFID support since they avoid the patent issues with Mifare, which requires an NXP chipset or license fee. These cards are further used to write a minimal JWT to it which then can be used to open the door.
14. **20 x 4 positive LCD with RGB** - The screen is an optional fun device which in my case is used to print the door state and which user it is that interacted with the door.
15. **I2C-interface for LCD** - The I2C interface is used to simplify communication when using the LCD screen, as you otherwise would have to connect a bunch of jumper cables individually onto the LCD screen pins.
16. **A pack of resistors** - The resistor values will vary depending on what we want to achieve, and it can be calculated according to Ohm's law. I used 220 Ω resistors for the lights, and a 2K Ω resistor for my TIP120 transistor. Generally resistors are useful as a safety measure when connecting the electronics with the breadboard.
17. **A pack of LEDs** - A pack of LED lights comes in handy when debugging, but in my case I also use them to reflect various states in my application. For example I might use a red light to indicate an error state inside the application. For example I might use an orange blinking light to indicate that the PN532 is currently reading the NFC card. I might have a solid orange light on to indicate that the NFC card should be written to, I might use a blue LED to indicate that the JWT was written to the NFC card successfully, and I might use a green LED to indicate that the door has been opened, and so on.
18. **Door knob** - A door knob comes in handy to provide a good grip in terms of opening/closing the door if necessary.
19. **2x Door hinge** - Door hinges are necessary to mount the door in place.
20. **Prototype door** - The door itself can be made of wood, plywood or a more simple hobby material.
21. **Glue** - Glue is recommended to connect the wooden parts of the door together.
22. **Soldering Iron** - A soldering iron is required as it is necessary to solder the FTDI cable with the PN532, and it was also needed to solder the version of the 20 x 4 LCD screen that I got.

Since I am located in Sweden, I could not order directly from Adafruit for instance since that would be very expensive and time consuming, and so I had to resort to various retailers nearby to find the parts I was looking for.

The material came from these seven places:

- elfa.se

  - PN532 NFC/RFID
  - NTAG203 13.56 MHz RFID/NFC-cards
  - 20 x 4 positive LCD with RGB
  - Breadboard

- electrokit.com

  - Buzzer
  - Solenoid 12V
  - FTDI-cable USB/TTL
  - Adapter DC-jack
  - 1N4007 DO-41 1000V 1A diode
  - I2C-interface for LCD

- m.nu

  - Breadboarding Female/Female Jumper Wires
  - Breadboarding Female/Male Jumper Wires
  - Breadboarding Male/Male Jumper Wires
  - TIP120 NPN Power Darlington Transistor

- shop.pimoroni.com

  - Raspberry Pi 4
  - A pack of resistors
  - A pack of LEDs

- hornbach.se

  - Door knob
  - 2x Door hinge
  - Prototype door
  - Glue

- kjell.com

  - Soldering Iron

- My home

  - 12V DC Power Adapter

All in all, this is quite a bit of material that I used to create this project and which costed about approximately 3000 SEK, but it's not set in stone and you might find yourself using other material to achieve more or less the same result at a cheaper price.

### Computer setup

TODO: How did I setup Raspberry Pi with the PN532?

I am using VS Code for my editor of choice, and when writing the code on my Raspberry Pi I found it useful to use RDP (Remote Desktop Protocol) or any kind of software that allows you to see the screen. Then in terms of uploading the code, I would setup SSH on the Raspberry Pi and upload the code to my Github for version management.

- [ ] Steps that you needed to do for your computer. Installation of Node.js, extra drivers, etc.

### Putting everything together

How is all the electronics connected? Describe all the wiring, good if you can show a circuit diagram. Be specific on how to connect everything, and what to think of in terms of resistors, current and voltage.

I would not consider myself to be an expert in terms of electronics as it is my first time having laid my hands on this, but I would like to believe that I followed best practices to ensure a more or less production ready setup more so than a development only setup..

![Fritzing Overview](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627771854/Erik/IoT%20Summer%20Course/IoT_Project_bb_oxqmlj.png)

- [ ] Circuit diagram (can be hand drawn)
- [ ] \*Electrical calculations

### Platform

In general, most of the services are running locally off the cloud which I found provided a great developer experience as well as a greater control over how each service is running. I also however tried out hosting all of the services separately, and that works fine as well except for the websocket issue which lead me into hosting the Next.js application locally as a custom server for the time being.

- **Postgres** - I use a hosted postgres solution from [Heroku](https://www.heroku.com) to manage my Prisma ORM, but it could just as well be hosted locally too for faster performance. It is just that if I were to deploy this on for example Vercel in the future, then it would be more convenient to have the Postgres database already hosted somewhere other than on your own computer for everything to go more smoothly.

- **Redis** - I currently run a Redis server from another Raspberry Pi 4 that I have laying around at home. However you can also go for a hosted solution in the form of [upstash](https://upstash.com). I would most likely choose this approach if I were to have my Next.js application served from Vercel.

- **Next.js application** - I currently run [Next.js](https://nextjs.org) as a custom server on my own computer. Hosting Next.js applications on [Vercel's](https://vercel.com) Global CDN is a great idea in general as it features among the worlds fastest performance, but since Vercel does not support websockets I decided to go with the custom server approach in this case. Other solutions would be to run GraphQL and subscriptions on a separate express application, and the Next.js application on another one, but since I combined GraphQL within my Next.js application, it just seemed like a better option would be to create a custom server and have everything in one place.

- **Raspberry Pi Node.js application** - The Raspberry Pi application that handles the electronics is portforwarded by my router so that it can be communicated with regardless of where the other services are hosted. This is the only application that I find a legitimate reason to only host locally due to the electronics involved etc.

To summarize; some parts are hosted on the cloud, and others are hosted locally.
However, any solution generally works and all of the services I use have got rather generous free tiers without any payments required. Should the application scale very well, then one would have to consider the cost of each service and act according to that.

### The code

There is a whole bunch of code and dependencies that makes up this project. But to keep it somewhat simple to understand, I will only be showing and explaining some key snippets from the application.

As for the code snippet below, I am awaiting the results from a getNFC function which I pass the unique ID of the NFC card to as an argument. This function will send off a GraphQL query to the Next.js application GraphQL Server, and then that will go through a so called resolver which queries the Prisma database for an NFC with the ID inside the argument, which is the ID of the NFC card. If the ID is not there, then it returns null.

```js
const getNFCResult = await getNFC(tag.uid);

if (!getNFCResult) {
  return null;
}
```

If on the other hand the NFC ID exists inside the Prisma database, but there is no user, then that card does not belong to the rightful user, and it will then print a message saying that the door can only be unlocked by someone else.

```js
if (getNFCResult.id && !getNFCResult.user) {
  turnOnThenOffDevice(redLED, 2000, () => {
    isCardBusy(false);
  });
  return console.log(
    chalk.hex("#AE3450")("The door can only be unlocked by someone else")
  );
}
```

However, if we do have a user, we will first of all clear any potential red LED, and then we will fire off the unlockDoor function. We will also print a message saying welcome, along with the user's email that opened the door.

```js
if (getNFCResult.user) {
  // Code that runs when the door is opened
  turnOffDevice(redLED);
  unlockDoor(getNFCResult.user);
  return console.log(
    chalk.hex("#68AA55")(`Welcome ${getNFCResult.user.email}!`)
  );
}
```

The asynchronous unlockDoor function below is what does quite a bit of magic.
First of all it will open up the solenoid for 5 seconds along with the green light, and then it will lock itself. The green LED will also shut off after 5 seconds. Inside the callback function however, it will fire off a publishDoorState function with the state being set to CLOSE along with the user information as arguments. This code will update the UI inside the client-side application to render the door closed, much like the state of the physical door.

Inside the try block in the case that a user was passed in to the unlockDoor function, what will happen is that an initMessage function will be called, which simply prints the user to a 20x4 LCD screen. After that, a POST request will be fired off with the state being set to OPEN, along with the user information as arguments - to a Next.js serverless function.

```js
export async function unlockDoor(user) {
  turnOnThenOffDevice(solenoidLock, 5000);
  turnOnThenOffDevice(greenLED, 5000, async () => {
    isCardBusy(false);
    await publishDoorState("CLOSE", user);
  });

  try {
    if (user) {
      initMessage(user.username);
      await publishDoorState("OPEN", user);
    }

    // Send message to Next.js Application to let it know that the door is open through a serverless function.
  } catch (error) {
    console.log(chalk.hex("#AE3450")("unlockDoor catch error", error));
  }
}
```

The Next.js serverless function will then run its logic containing a switch statement to see if any of the passed in door state matches.

```js
switch (doorState) {
  case "OPEN":
    message = "Door unlocked";
    doorStatus = "DOOR_OPEN";

    pubsub.publish("DOOR_OPENED", {
      status: doorStatus,
      message,
      user: username,
    });
    break;
  case "CLOSE":
    message = "Door locked";
    doorStatus = "DOOR_CLOSED";

    pubsub.publish("DOOR_CLOSED", {
      status: doorStatus,
      message,
      user: username,
    });
    break;
  case "TOGGLE":
    message = "Door temporarily unlocked";
    doorStatus = "DOOR_TEMPORARILY_UNLOCKED";

    pubsub.publish("DOOR_TEMPORARILY_UNLOCKED", {
      status: doorStatus,
      message,
      user: username,
    });
    break;
  default:
    console.log("publishDoor default condition - invalid door state");
}
```

If for example the door state OPEN is passed in, then the DOOR_OPENED event will be published. Once published, there is a subscriber on the other end at the client-side of the Next.js application which subscribes to this published event.
What then happens is that the UI is rerendered based on the information inside the published event, causing the UI in the client-side application to reflect the state of the physical door, and by what user it was that interacted with the door.

### Transmitting data and connectivity

I am using an ethernet connection for all of my locally hosted services where data is sent fairly frequently.
Communication happens via the HTTPS protocol as well as via webhooks and websockets through GraphQL subscriptions.
Furthermore, GraphQL queries, mutations and subscriptions, as well as serverless functions are used - which utilizes API routes inside Next.js - to handle communication between my services.

One great example of this is where the Raspberry Pi Node.js application will send a POST request to the Next.js API route once the door has been interacted with. This Next.js API route is using a serverless function which is on the backend side of things, and this serverless function will then use the publish-subscribe pattern to publish an event where the door has been interacted with, and then the client will with the help of subscriptions subscribe to this published event which would then render the UI according to the state of this published event. The serverless function also happens to send off a webhook event to Discord, which also would let me know the state of the door and by who it was opened.

Things like device range and battery consumption was of no use to me for this particular project, and so I did not have to think about this aspect.

I did look into other protocols as well where one intriguing one was MQTT. I could have used the NPM library called graphql-mqtt-subscriptions to handle GraphQL subscriptions. I could use this to also publish and subscribe to certain things - but in the end I did not see a need for my particular use case, and instead I went with a pubsub approach using the NPM library called graphql-redis-subscriptions which instead utilizes Redis; which is yet another production ready library for dealing with GraphQL subscriptions using websockets.

### Presenting the data

Both the Raspberry Pi Node.js server and the Next.js application communicate to each other with every write and read to the NFC card to ensure that the JWT is still saved inside Redis for instance.
I try to minimize database calls wherever I can and instead utilize Redis whenever possible as Redis as an in-memory data store tends to be quite a bit faster in terms of performance compared to interacting with a regular database.

The unique ID of the NFC card is saved and connected to a user inside my Prisma ORM that uses Postgres under the hood upon every write. However, the JWT that has been written to the NFC card is stored inside Redis only for 5 minutes before expiring, and then one would have to issue and write another JWT onto the NFC card to be able to interact with the door once again.

GraphQL queries, mutations and subscriptions are cached with a SHA-256 hash and are stored inside Redis with something known as persisted queries. Persisted queries are not truly related to the IoT side of things, but it does help with performance and improve network performance, particularly when it comes to larger query strings. Clients can then send the identifier instead of the corresponding query string, thus reducing request sizes dramatically.

- [ ] Provide visual examples on how the dashboard looks. Pictures needed.
- [ ] How often is data saved in the database.
- [ ] \*Explain your choice of database.
- [ ] \*Automation/triggers of the data.

### Closing thoughts

I achieved everything I could possibly want for this project, and I am very much content with the results.
Further down the line I plan on adding on further datasources to my GraphQL gateway, where I can leverage the IoT related things for other projects as well. In that sense it feels great to have all the infrastructure already in place with a good architecture that allows you to easily add on more datasources to my GraphQL gateway and what not.

Lastly I would like to conclude that a single Node.js API for the Raspberry Pi is more than likely all you would need to get going with this type of project.
In fact, initially I let the Raspberry Pi Node.js API handle everything from the IoT side of things to authentication and to creating users with a MongoDB database. Later on however, I thought it would be a better idea to turn this into a microservice architecture to adhere to greater separation of concerns and for greater scalability in the future.
By going this route, the Raspberry Pi Node.js API would then only handle things related to the IoT and electronic side of things, such as reading and writing to the NFC card, turning on and off lights, and unlocking the door. In other words, it would no longer have to deal with things such as authentication or having its own database and so on. Instead, this is why I decided to create a separate Next.js application that also uses GraphQL which then adds on the RESTful Raspberry Pi API as a datasource on top of the GraphQL layer, so that this application can deal with authentication and distributing JSON Web Keys for other microservices to consume and what not.

TODO: Insert pictures

- [ ] Show final results of the project
- [ ] Pictures
- [ ] \*Video presentation
