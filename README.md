## Authentication system featuring a prototype door, NFC technology and JSON Web Tokens

#### Student Name: Erik Claesson
#### Student ID: ec222qs

### Overview

In this project I will go through how I built my own authentication system featuring a door that can be opened with an NFC card along with the help of a JSON Web Token (JWT) stored onto it for extra security.

This project also briefly goes over my technologies of choice, such as Next.js, GraphQL, Prisma and Redis, etc and how I used these technologies together in a microservice architecture.

I will provide a brief overview but without going into too much detail here about my choice of technologies and what they are:

1. **Next.js**, the React Framework, is a solution to build hybrid applications built with static assets and serverless functions. It is essentially a fullstack framework that gives you a backend and a frontend to work with.
2. **GraphQL** is a query language for your API, and a server-side runtime for executing queries using a type system that you can define for your data, and this is arguably a better approach compared to something more traditional like REST.
3. **Prisma** is a Next-generation Node.js and TypeScript ORM which I use Postgres with. With Prisma, you define your models in the declarative Prisma schema which serves as the single source of truth for your database schema and the models in your programming language of choice.
4. **Redis** is an in-memory data structure store which I use as a message broker through the publish-subscribe-pattern, as well as caching GraphQL queries with persisted queries, and storing JSON Web Tokens.

![architecture-overview](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627939716/Erik/IoT%20Summer%20Course/IoT_Summer_Project_-_Architecture_nyioht.jpg)

##### Estimated time: 60+ hours - depends greatly on knowledge and familiarity.
##### Estimated price: 3000 SEK

### Objective

My goal when building this project was to do something that involved interacting with a door from a NFC card, and even better if JSON Web Tokens could be involved with this I figured. Another goal of mine was to achieve all of this with JavaScript as that is my primary language, but also due to the fact that it is less commonly used together with IoT which I saw as a challenge in itself.

The idea of combining software written on the computer with electronics in real life is also something that was incredible to me, and so it has been quite the journey all the way from start to finish with this project.

In the end, the purpose and insights of this project has mainly been educational and for myself to see what code and electronics can achieve together when combined. There is however also a valid use case for this as I definitely can see the need for more smart authentication systems in the near future.

### Material

The material used for this project includes:

| Material                                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Cost (SEK) | Store                      |
|------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|----------------------------|
| Raspberry Pi 4 4GB                       | In my project I decided to use a Raspberry Pi 4 to spin up a Node.js server. By leveraging a couple of NPM libraries I could then communicate with any lights, buzzers, the solenoid, the PN532, and so on.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 690        | https://shop.pimoroni.com  |
| 3x Breadboard                            | At least one breadboard should be required to wire up all the electronics.  In my case I used three different breadboards for better separation of concerns.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 150        | https://www.elfa.se        |
| Breadboarding Female/Female Jumper Wires |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 60         | https://www.m.nu           |
| Breadboarding Female/Male Jumper Wires   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 60         | https://www.m.nu           |
| Breadboarding Male/Male Jumper Wires     | I used the jumper cables to connect the GPIO pins to the breadboard.  I also used jumpers from the breadboard to the solenoid and to the DC-jack                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | 60         | https://www.m.nu           |
| TIP120 NPN Power Darlington Transistor   | The transistor is used to drive various devices that have high power requirements, such as the 12V solenoid.  This is necessary since I do not want to directly interact with the solenoid through my Raspberry Pi.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 29         | https://www.m.nu           |
| Adapter DC-jack                          | The DC-jack is used to power up the 12V solenoid with the help of a 12V DC Power Adapter.  One could also use batteries or a powerbank for this which would provide better mobility if needed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 20         | https://www.electrokit.com |
| 1N4007 DO-41 1000V 1A diode              | The diode is used to allow flow of current only in one direction.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | 2.40       | https://www.electrokit.com |
| Buzzer                                   | The buzzer creates a beeping noise. I use this for the cases when the JWT has been written to the NFC card, or when the door has been interacted with.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 29         | https://www.electrokit.com |
| Solenoid 12V                             | The solenoid is essentially an electronic lock which in my case is used as the lock mechanism for the door.  When 9-12VDC is applied, the slug pulls in so it doesn't stick out anymore and the door can then be opened.  Draws 650mA at 12V, 500 mA at 9V when activated.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 199        | https://www.electrokit.com |
| 12V DC Power Adapter                     | I had a generic 12V DC Power Adapter laying around, and so I used this to power the DC-jack which in turn would give power to the solenoid.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 0          | My Residence               |
| FTDI-cable USB/TTL                       | The FTDI-cable is used to convert TTL serial transmissions to and from USB signals which in my case results in data being sent to the Raspberry Pi from the PN532 device.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 279        | https://www.electrokit.com |
| PN532 NFC/RFID                           | The PN532 is a popular device which allows reading and writing to for example an NFC card which in turn will interact with the door.  The device can be used via I2C, UART or SPI communication protocols.  In my case I used the UART protocol for simplicity.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 376        | https://www.elfa.se        |
| 4x NTAG203 13.56 MHz RFID/NFC-cards      | These NFC cards are often used as train or bus passes.  The tag contains a small RFID chip and an antenna, and is passively powered by the reader/writer when placed a couple inches away. A couple of things to note here is that these chips can be written to and store only up to 144 bytes of data in writable EEPROM divided into 4 byte banks divided in 36 pages.  This is not enough for a large sized JWT, but it is enough for many use cases nevertheless.  Another thing to note is that unlike "Classic 1K" cards, these tags are more secure and work with almost any phone with RFID support. This is because they avoid the patent issues with Mifare, which requires an NXP chipset or license fee.  These cards are further used to write a minimal JWT to it which then can be used to open the door. | 112.40     | https://www.elfa.se        |
| 20 x 4 positive LCD with RGB             | The screen is an optional fun device which in my case is used to print the door state and which user it is that interacted with the door.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 238        | https://www.elfa.se        |
| I2C-interface for LCD                    | The I2C interface is used to simplify communication when using the LCD screen, as you otherwise would have to connect a bunch of jumper cables individually onto the LCD screen pins.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 39         | https://www.electrokit.com |
| A pack of resistors                      | The resistor values will vary depending on what we want to achieve, and it can be calculated according to Ohm's law.  I used 220 Ω resistors for the lights, and a 2K Ω resistor for my TIP120 transistor.  Generally resistors are useful as a safety measure when connecting the electronics with the breadboard.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 90         | https://shop.pimoroni.com  |
| A pack of LED                            | A pack of LED lights comes in handy when debugging, but in my case I also use them to reflect various states in my application.  For example, I might use a red light to indicate an error state inside the application.  I might use an orange blinking light to indicate that the PN532 is currently reading the NFC card.  I might have a solid orange light on to indicate that the NFC card should be written to. I might use a blue LED to indicate that the JWT was written to the NFC card successfully, and I might use a green LED to indicate that the door has been opened, and so on.                                                                                                                                                                                                                        | 80         | https://shop.pimoroni.com  |
| Door knob                                | A door knob comes in handy to provide a good grip in terms of opening and closing the door if necessary.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 30         | https://www.hornbach.se    |
| 2x Door hinge                            | Door hinges are necessary to mount the door in place.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 50         | https://www.hornbach.se    |
| Prototype door                           | The door itself can be made of wood, plywood or a more simple hobby material.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 120        | https://www.hornbach.se    |
| Glue                                     | Glue is recommended to connect the wooden parts of the door together.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 0          | My Residence               |
| Soldering Iron                           | A soldering iron is required as it is necessary to solder the FTDI cable with the PN532, and it was also needed to solder the version of the 20 x 4 LCD screen that I got.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 99         | https://www.kjell.com/se   |

All in all, this is quite a bit of material that I used to create this project and which costed around approximately 3000 SEK, but the parts to make this project come true is not set in stone and you might find yourself using other material to achieve more or less the same outcome at a cheaper price.

### Computer setup

In order to make efficient use of the Raspberry Pi, I personally like to install RDP (Remote Desktop Protocol). This is because I like to only sit with one computer instead of going back and forth between the two.

In order to make RDP work, we must first make sure that the Raspberry Pi is up to date.
We can ensure this by running the following command - which will update our Raspberry Pi to the latest version:
```
sudo apt update && sudo apt upgrade
```
Once that is done, we can go ahead and install the xrdp package. 
This package will allow remote desktop on the Raspberry Pi.
```
sudo apt install xrdp
```
Also, in case we need the IP address of the Raspberry Pi, we can use this handy command:
```
hostname -I
```
Next up, we can open Remote Desktop Connection on our other computer and we should be able to see a screen like this:

![Remote Desktop Image](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627846033/Erik/IoT%20Summer%20Course/Screenshot_847_z3ctdo.png)

After this we will install Node.js on our Raspberry Pi, and we can do that by heading over to the Node.js website and grabbing the latest version for ARMv7. 
We can use wget to download the file like this:
```
wget https://nodejs.org/dist/v14.17.4/node-v14.17.4-linux-armv7l.tar.xz
```
And to pack it up:
```
tar xf node-v14.17.4-linux-armv7l.tar.xz node-v14.17.4-linux-armv7l/
```
Then we head into the directory and copy all of the files inside /usr/local/ with the following command:
```
cp -R * /usr/local/
```
We can now check if everything went well by running the following checks:
```
node -v
npm -v
```

Once we have got Node.js installed, we should be able to start up the app, but we need to take some additional steps to really get PN532 working. 
It is advisable to also install some further packages from libnfc so that we can run some nfc commands to easier debug and so on.

We can do that with the following command:
```
sudo apt install libnfc-bin libnfc-examples
```
Now we should be able to run commands such as nfc-poll and nfc-scan-device

We also must configure some settings for our Raspberry Pi, and we can do that by running the following command:
```
sudo raspi-config
```
While we are here, we can toggle ```Enable I2C ``` which we will need for the LCD Screen.
We can also toggle ```Enable Serial Port``` and ```Disable Serial Console``` to disable shell and kernel messages via UART. 
Next step is to enable UART, and for that we have to open and write to a file. We can do that with the following command:
```
sudo nano /boot/config.txt
```

We can then enable UART by adding the following line (if it is not already present):
```
enable_uart=1
```
After that, we will go ahead and edit the nfc libnfc configuration file:
```
sudo nano /etc/nfc/libnfc.conf
```
We will add the following line:
```
device.connstring = "pn532_uart:/dev/ttyUSB0"
```
Once done, we should be able to run the following command and see that the NFC device has been found:
```
nfc-scan-device -v
```
![nfc-scan-device](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627850234/Erik/IoT%20Summer%20Course/Screenshot_851_upqs3k.png)

At this point, we should be able to read and write with the PN532 NFC device as well as being able to use the LCD screen with I2C!

### Putting everything together

In terms of putting everything together, you may refer to my fritzing schematic below. 
One key ingredient here for me was reading up on the product documentation for the various devices to better understand how I would wire everything up, and how I could communicate with the devices programatically.

One example is my LCD screen which was a newer version that had some extra features, including support for red, green and blue, while the I2C interface did not. However, once I read through the product documentation for the screen, I got an idea of how I could utilize the I2C interface to still get the colors that I wanted by wiring it up slightly differently and by not letting the I2C dictate the color in which case I would only have had red color.

#### Fritzing Schematic

![Fritzing Overview](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627934256/Erik/IoT%20Summer%20Course/IoT_Project_bb_h5ibgz.png)

### Platform

In general, most of the services are running locally off the cloud which I found provided a great developer experience as well as a greater control over how each service is running. I also however tried out hosting all of the services separately, and that works fine as well except for the websocket issue which lead me into hosting the Next.js application locally as a custom server for the time being.

- **Postgres** - I use a hosted postgres solution from [Heroku](https://www.heroku.com) to manage my Prisma ORM, but it could just as well be hosted locally too for faster performance. It is just that if I were to deploy this on for example Vercel in the future, then it would be more convenient to have the Postgres database already hosted somewhere other than on your own computer for everything to go more smoothly.

- **Redis** - I currently run a Redis server from another Raspberry Pi 4 that I have laying around at home. However you can also go for a hosted solution in the form of [upstash](https://upstash.com). I would most likely choose this approach if I were to have my Next.js application served from Vercel.

- **Next.js application** - I currently run [Next.js](https://nextjs.org) as a custom server on my own computer. Hosting Next.js applications on [Vercel's](https://vercel.com) Global CDN is a great idea in general as it features among the worlds fastest performance, but since Vercel does not support websockets I decided to go with the custom server approach in this case. Other solutions would be to run GraphQL and subscriptions on a separate express application, and the Next.js application on another one, but since I combined GraphQL within my Next.js application, it just seemed like a better option would be to create a custom server and have everything in one place.

- **Raspberry Pi Node.js application** - The Raspberry Pi application that handles the electronics is portforwarded by my router so that it can be communicated with regardless of where the other services are hosted. This is the only application that I find a legitimate reason to only host locally due to the electronics involved etc.

To summarize; some parts are hosted on the cloud, and others are hosted locally.
However, any solution generally works and all of the services I use have got rather generous free tiers without any payments required. Should the application scale very well, then one would have to consider the cost of each service and act according to that.

### IoT specific libraries

We can make use of libraries so that we can communicate with certain hardware and sensors through a programming language like JavaScript as in my case. The following libraries were used specific to IoT:

| Library                    | Description                                                                                                                                           | Link                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| raspberrypi-liquid-crystal | Library to communicate with the LCD screen                                                                                                            | https://www.npmjs.com/package/raspberrypi-liquid-crystal |
| onoff                      | Library compatible with the GPIO pins of the Raspberry Pi. Allows us to power on and off anything we can think of such as lights, solenoids, etc.     | https://www.npmjs.com/package/onoff                      |
| pn532                      | Library to communicate with the pn532 device                                                                                                          | https://www.npmjs.com/package/pn532                      |
| serialport                 | Library to access serial ports with JavaScript                                                                                                        | https://www.npmjs.com/package/serialport                 |
| ndef                       | Library to create and parse NDEF messages. NDEF is a data exchange format which can be used to exchange information between any compatible NFC device | https://www.npmjs.com/package/ndef                       |

### The code

Many dependencies make up this project. But to keep it somewhat simple to understand, I will only be showing and explaining some key snippets from the application.

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

#### Choice of database

I choose to use Postgres as my database of choice, with Prisma as the ORM. I choose this type of database and ORM because I want the data to be structured and because Prisma in particular provides a clean and type-safe API with an enjoyable syntax overall. Using Prisma as an ORM abstracts away the SQL by letting me define my application models as classes, and these classes are mapped to tables in the database. You can then easily read and write data by calling methods on the instances of the model classes. I found this approach made me more productive and a way for me to develop quickly.

#### Microservice communication

In terms of communication between my services, both the Raspberry Pi Node.js server and the Next.js application communicate to each other with every write and read to the NFC card to ensure that the JWT is still saved inside Redis for instance.
I try to minimize database calls wherever I can and instead utilize Redis whenever possible as Redis as an in-memory data store tends to be quite a bit faster in terms of performance compared to interacting with a regular database.

The unique ID of the NFC card is saved and connected to a user inside my Prisma ORM that uses Postgres under the hood upon every write. However, the JWT that has been written to the NFC card is stored inside Redis only for 5 minutes before expiring, and then one would have to issue and write another JWT onto the NFC card to be able to interact with the door once again.

Below is an attached picture which shows the JWT of the NFC card stored inside of Redis.

![redis-nfc-token](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627923035/Erik/IoT%20Summer%20Course/Screenshot_854_ihbe1n.png)

GraphQL queries, mutations and subscriptions are cached with a SHA-256 hash and are stored inside Redis with something known as persisted queries. Persisted queries are not truly related to the IoT side of things, but it does help with performance and improve network performance, particularly when it comes to larger query strings. Clients can then send the identifier instead of the corresponding query string, thus reducing request sizes dramatically.

Below is an attached picture which shows a saved peristed query inside of Redis.

![persisted-query](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627923296/Erik/IoT%20Summer%20Course/Screenshot_856_sco9dd.png)

### Conclusion

I achieved everything I could possibly want for this project, and I am very much content with the results.
Further down the line I plan on adding on further datasources to my GraphQL gateway, where I can leverage the IoT related hardware for other projects as well. In that sense it feels great to have all the infrastructure already in place with a solid architecture that allows you to easily add on more datasources to the GraphQL gateway and what not.

Below are some pictures of the project in action!

![door](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627921245/Erik/IoT%20Summer%20Course/IMG_20210802_181759_zgaxql.jpg)

![behind-door](https://res.cloudinary.com/cubbans-cloud/image/upload/q_60/v1627921282/Erik/IoT%20Summer%20Course/IMG_20210802_014543_y9wr9q.jpg)

![door-open-nfc](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627928458/Erik/IoT%20Summer%20Course/IMG_20210802_201935_woy8uh.jpg)

![welcome-erik](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627928462/Erik/IoT%20Summer%20Course/IMG_20210802_201844_jecb5u.jpg)

![door-open](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627922467/Erik/IoT%20Summer%20Course/Screenshot_852_awznm8.png)

![door-closed](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627922467/Erik/IoT%20Summer%20Course/Screenshot_853_sl1fap.png)

![card-written](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627922949/Erik/IoT%20Summer%20Course/IMG_20210802_182556_atodry.jpg)

![login](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627923238/Erik/IoT%20Summer%20Course/Screenshot_857_ramfna.png)

![issue-card](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627923238/Erik/IoT%20Summer%20Course/Screenshot_858_rbcjjn.png)

![admin-interact-with-door](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627923238/Erik/IoT%20Summer%20Course/Screenshot_859_ielokg.png)

![discord-alert](https://res.cloudinary.com/cubbans-cloud/image/upload/v1627922557/Erik/IoT%20Summer%20Course/Screenshot_855_vf7bx9.png)

Lastly I would like to just mention that a single Node.js API for the Raspberry Pi is more than likely all you would need to get going with this type of project.
In fact, initially I let the Raspberry Pi Node.js API handle everything from the IoT side of things to authentication and to creating users with a MongoDB database. Later on however, I thought it would be a better idea to turn this into a microservice architecture to adhere to greater separation of concerns and for greater scalability in the future.
By going this route, the Raspberry Pi Node.js API would then only handle things related to the IoT and electronic side of things, such as reading and writing to the NFC card, turning on and off lights, and unlocking the door. In other words, it would no longer have to deal with things such as authentication or having its own database and so on. Instead, this is why I decided to create a separate Next.js application that also uses GraphQL which then adds on the RESTful Raspberry Pi API as a datasource on top of the GraphQL layer, so that this application can deal with authentication and distributing JSON Web Keys for other microservices to consume and what not.

PS. Video coming!
