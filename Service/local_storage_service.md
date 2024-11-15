# 🚀 LocalStorage Service

** Class Local Storage Service **

## Create localStorageService.ts file

```ts
export type StoredItem<T> = {
  id: string;
  title: string;
  expiration?: number;
  data: T;
};

export default class LocalStorageService {
  private static PREFIX = 'misa_';

  private static getStorageKey(key: string): string {
    return `${this.PREFIX}${key}`;
  }

  public static setItem<T>(key: string, data: T, expirationInMs: number | undefined = undefined, title?: string): void {
    const id = Date.now().toString();
    const expiration = expirationInMs ? Date.now() + expirationInMs : undefined;

    const item: StoredItem<T> = {
      id,
      title: title ? title : key,
      expiration,
      data,
    };

    try {
      localStorage.setItem(this.getStorageKey(key), JSON.stringify(item));
    } catch (error) {
      console.error('Error saving to local storage', error);
    }
  }

  public static getItem<T>(key: string, fullBody = false): StoredItem<T> | T | null {
    const item = localStorage.getItem(this.getStorageKey(key));
    if (!item) return null;

    try {
      const parsedItem: StoredItem<T> = JSON.parse(item);

      console.log('expiration: ', parsedItem.expiration);
      console.log('now: ', Date.now());
      console.log(parsedItem.expiration && parsedItem.expiration < Date.now());
      if (parsedItem.expiration && parsedItem.expiration < Date.now()) {
        this.removeItem(key);
        return null;
      }

      return fullBody ? parsedItem : parsedItem.data;
    } catch (error) {
      console.error('Error reading from local storage', error);
      return null;
    }
  }

  public static removeItem(key: string): void {
    try {
      localStorage.removeItem(this.getStorageKey(key));
    } catch (error) {
      console.error('Error removing from local storage', error);
    }
  }

  public static clearAll(): void {
    try {
      localStorage.clear();
    } catch (error) {
      console.error('Error clearing local storage', error);
    }
  }
}
```
