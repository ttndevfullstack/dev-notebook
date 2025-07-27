# Node Logger Application

---

## \_A list library to log information for the application

### Installation

_Below is an example of how you can instruct your audience on installing and setting up logger in NodeJS. This template doesn't rely on any external dependencies or services._

1. Install NPM packages
   ```sh
   npm install
   ```
2. Add this config to `config/logger.js`

   ```sh
   const winston = require("winston");

    const logger = winston.createLogger({
      level: "info",
      format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.printf(
          (info) => `${info.timestamp} ${info.level}: ${info.message}`
        )
      ),
      transports: [
        new winston.transports.Console(),
        new winston.transports.File({ filename: "logs/app.log" }),
      ],
    });

    module.exports = logger;
   ```

3. Create a logs/app.log file at root
   ```sh
   mkdir logs && touch logs/app.log
   ```
