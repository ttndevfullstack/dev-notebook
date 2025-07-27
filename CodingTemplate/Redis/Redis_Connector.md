# 🚀 Base Service Redis

**🌐 A list method for redis cache strategy 🌐**

---

## Setup

All functions in Redis cache with RedisService class._

```sh
const redis = require('redis');
const logger = require('../config/logger');

class RedisService {
constructor() {
 this.client = null;
}

validateEnvVars() {
 const redisHost = process.env.REDIS_HOST;
 const redisPort = process.env.REDIS_PORT;

 if (!redisHost) {
   throw new Error('REDIS_HOST environment variable is missing');
 }
 if (!redisPort) {
   throw new Error('REDIS_PORT environment variable is missing');
 }

 return { redisHost, redisPort };
}

async connect() {
 try {
   const { redisHost, redisPort } = this.validateEnvVars();

   if (this.client && this.client.isOpen) {
     logger.info('Reusing existing Redis connection');
     return this.client;
   }

   this.client = redis.createClient({
     url: `redis://${redisHost}:${redisPort}`,
   });

   this.client.on('error', (err) => logger.error('Redis Client Error: ', err));
   this.client.on('connect', () => logger.info('Redis client connected'));
   this.client.on('reconnecting', () => logger.warn('Redis client reconnecting'));
   this.client.on('end', () => logger.warn('Redis client disconnected'));

   await this.client.connect();
   logger.info('Connected to Redis successfully');

   return this.client;
 } catch (error) {
   logger.error('Failed to connect to Redis:', error);
   throw error;
 }
}

async disconnect() {
 try {
   if (this.client && this.client.isOpen) {
     await this.client.quit();
     logger.info('Redis client disconnected successfully');
   }
 } catch (error) {
   logger.error('Error while disconnecting Redis client', error);
   throw error;
 }
}

async get(key) {
 if (!key) throw new Error('Key is required');

 try {
   return await this.client.get(key);
 } catch (error) {
   logger.error(`Error fetching value for key: ${key}`, error);
   throw error;
 }
}

async setExpire(key, value, expiration) {
 if (!key || !value) throw new Error('Key and value are required');
 if (!expiration) throw new Error('Expiration is required');

 try {
   return await this.client.setEx(key, expiration, value);
 } catch (error) {
   logger.error(`Error setting value for key: ${key}`, error);
   throw error;
 }
}

async delete(key) {
 if (!key) throw new Error('Key is required');

 try {
   return await this.client.del(key);
 } catch (error) {
   logger.error(`Error deleting key: ${key}`, error);
   throw error;
 }
}

async exists(key) {
 if (!key) throw new Error('Key is required');

 try {
   const exists = await this.client.exists(key);
   return exists === 1;
 } catch (error) {
   logger.error(`Error checking existence of key: ${key}`, error);
   throw error;
 }
}

async increment(key) {
 if (!key) throw new Error('Key is required');

 try {
   return await this.client.incr(key);
 } catch (error) {
   logger.error(`Error incrementing value for key: ${key}`, error);
   throw error;
 }
}
}

module.exports = new RedisService();

```
