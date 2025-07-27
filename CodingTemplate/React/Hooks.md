# 🚀Top 10 Custom Hooks for Every React Developer

**🌐 React’s custom hooks allow developers to reuse logic across components, making code more maintainable, modular, and easier to read 🌐**

### useFetch

--- The useFetch hook is commonly used to fetch data from an API. It manages loading, error, and response states, encapsulating all the logic needed for data retrieval.

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
	const [data, setData] = useState(null);
	const [loading, setLoading] = useState(true);
	const [error, setError] = useState(null);

	useEffect(() => {
		const fetchData = async () => {
			try {
				const response = await fetch(url);
				const result = await response.json();
				setData(result);
			} catch (err) {
				setError(err);
			} finally {
				setLoading(false);
			}
		};
		fetchData();
	}, [url]);

	return { data, loading, error };
}

// Usage:
const { data, loading, error } = useFetch('https://api.example.com/data');
```

### usePrevious

--- usePrevious saves the previous value of a state or prop, which can be handy for animations, form data, or complex state management.

```javascript
import { useRef, useEffect } from 'react';

function usePrevious(value) {
  const ref = useRef();

  useEffect(() => {
    ref.current = value;
  }, [value]);

  return ref.current;
}

Usage:
const previousCount = usePrevious(count);
```

### useToggle

--- A straightforward custom hook, useToggle manages boolean states efficiently, useful for toggling themes, modals, and UI elements.

```javascript
import { useState } from 'react';

function useToggle(initialValue = false) {
	const [value, setValue] = useState(initialValue);

	const toggle = () => setValue((prev) => !prev);

	return [value, toggle];
}

// Usage:
const [isToggled, toggle] = useToggle();
```

### useLocalStorage

--- useLocalStorage helps in setting, getting, and updating values stored in the browser’s local storage, enabling persistent data storage.

```javascript
import { useState } from 'react';

function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.log(error);
      return initialValue;
    }
  });

  const setValue = value => {
    try {
      setStoredValue(value);
      localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.log(error);
    }
  };

  return [storedValue, setValue];
}

Usage:
const [name, setName] = useLocalStorage('name', 'Guest');
```

### useDebounce

--- Debouncing prevents rapid triggering of events. useDebounce can be useful for input fields or search bars.

```javascript
import { useState, useEffect } from 'react';

function useDebounce(value, delay) {
	const [debouncedValue, setDebouncedValue] = useState(value);

	useEffect(() => {
		const handler = setTimeout(() => {
			setDebouncedValue(value);
		}, delay);

		return () => clearTimeout(handler);
	}, [value, delay]);

	return debouncedValue;
}

// Usage:
const debouncedSearchTerm = useDebounce(searchTerm, 500);
```

### useInterval

--- useInterval manages recurring actions by setting up intervals in a way that integrates well with React's lifecycle.

```javascript
import { useEffect, useRef } from 'react';

function useInterval(callback, delay) {
	const savedCallback = useRef();

	useEffect(() => {
		savedCallback.current = callback;
	}, [callback]);

	useEffect(() => {
		if (delay !== null) {
			const id = setInterval(() => savedCallback.current(), delay);
			return () => clearInterval(id);
		}
	}, [delay]);
}

// Usage:
useInterval(() => {
	setCount((prevCount) => prevCount + 1);
}, 1000);
```

### useWindowSize

--- useWindowSize is helpful for responsive design, allowing your component to adapt based on the viewport size.

```javascript
import { useState, useEffect } from 'react';

function useWindowSize() {
	const [windowSize, setWindowSize] = useState({
		width: window.innerWidth,
		height: window.innerHeight,
	});

	useEffect(() => {
		const handleResize = () => {
			setWindowSize({
				width: window.innerWidth,
				height: window.innerHeight,
			});
		};
		window.addEventListener('resize', handleResize);
		return () => window.removeEventListener('resize', handleResize);
	}, []);

	return windowSize;
}

// Usage:
const { width, height } = useWindowSize();
```

### useOnClickOutside

--- This hook is useful for closing modals, dropdowns, or any component that should close when clicking outside of it.

```javascript
import { useEffect } from 'react';

function useOnClickOutside(ref, handler) {
	useEffect(() => {
		const listener = (event) => {
			if (!ref.current || ref.current.contains(event.target)) return;
			handler(event);
		};
		document.addEventListener('mousedown', listener);
		document.addEventListener('touchstart', listener);
		return () => {
			document.removeEventListener('mousedown', listener);
			document.removeEventListener('touchstart', listener);
		};
	}, [ref, handler]);
}

// Usage:
const modalRef = useRef();
useOnClickOutside(modalRef, () => setIsOpen(false));
```

### useHover

--- Detecting hover over an element can be useful for tooltips, animations, or any UI feedback based on hover state.

```javascript
import { useState, useRef, useEffect } from 'react';

function useHover() {
	const [hovered, setHovered] = useState(false);
	const ref = useRef(null);

	useEffect(() => {
		const handleMouseOver = () => setHovered(true);
		const handleMouseOut = () => setHovered(false);
		const node = ref.current;
		if (node) {
			node.addEventListener('mouseover', handleMouseOver);
			node.addEventListener('mouseout', handleMouseOut);
		}
		return () => {
			if (node) {
				node.removeEventListener('mouseover', handleMouseOver);
				node.removeEventListener('mouseout', handleMouseOut);
			}
		};
	}, [ref]);

	return [ref, hovered];
}

// Usage:
const [hoverRef, isHovered] = useHover();
```

### useMediaQuery

--- useMediaQuery listens to CSS media queries, allowing you to apply specific styles or behaviors based on device size.

```javascript
import { useEffect, useState } from 'react';

function useMediaQuery(query) {
	const [matches, setMatches] = useState(false);

	useEffect(() => {
		const mediaQuery = window.matchMedia(query);
		setMatches(mediaQuery.matches);

		const handler = (event) => setMatches(event.matches);
		mediaQuery.addEventListener('change', handler);

		return () => mediaQuery.removeEventListener('change', handler);
	}, [query]);

	return matches;
}

// Usage:
const isLargeScreen = useMediaQuery('(min-width: 1024px)');
```

### useWebSocket

--- The useWebSocket hook connects your app to a WebSocket server, making it easy to send and receive real-time data. It handles reconnections, message buffering, and event-based handling, making it ideal for applications like chat apps, live notifications, or real-time data updates.

--- This example demonstrates a reusable WebSocket hook with features for connection management, event handling, and graceful cleanup.

```javascript
import { useState, useEffect, useRef, useCallback } from 'react';

function useWebSocket(url, options = {}) {
	const { reconnect = true, reconnectInterval = 5000, onOpen, onMessage, onError, onClose } = options;
	const [isConnected, setIsConnected] = useState(false);
	const [lastMessage, setLastMessage] = useState(null);
	const websocketRef = useRef(null);
	const reconnectTimeout = useRef(null);

	const connect = useCallback(() => {
		websocketRef.current = new WebSocket(url);

		websocketRef.current.onopen = (event) => {
			setIsConnected(true);
			onOpen && onOpen(event);
		};

		websocketRef.current.onmessage = (event) => {
			setLastMessage(event.data);
			onMessage && onMessage(event);
		};

		websocketRef.current.onerror = (event) => {
			onError && onError(event);
		};

		websocketRef.current.onclose = (event) => {
			setIsConnected(false);
			onClose && onClose(event);
			if (reconnect) {
				reconnectTimeout.current = setTimeout(connect, reconnectInterval);
			}
		};
	}, [url, reconnect, reconnectInterval, onOpen, onMessage, onError, onClose]);

	const sendMessage = useCallback(
		(message) => {
			if (isConnected && websocketRef.current) {
				websocketRef.current.send(message);
			}
		},
		[isConnected],
	);

	useEffect(() => {
		connect();

		return () => {
			if (websocketRef.current) {
				websocketRef.current.close();
			}
			clearTimeout(reconnectTimeout.current);
		};
	}, [connect]);

	return { isConnected, sendMessage, lastMessage };
}

// Features of useWebSocket
// Reconnection Handling: If the connection closes, the hook automatically tries to reconnect after a specified interval.
// Event Handling: Accepts callbacks for onOpen, onMessage, onError, and onClose events to handle them as needed.
// Message Sending: Provides a sendMessage function that only sends messages when the WebSocket is open.
// Last Message Storage: Stores the latest message received, allowing easy access to the most recent data without re-subscribing.

// Usage Example
// Here’s how to use useWebSocket in a component, such as a live chat or a real-time notification system.

function ChatApp() {
	const { isConnected, sendMessage, lastMessage } = useWebSocket('ws://localhost:4000/chat', {
		reconnect: true,
		reconnectInterval: 3000,
		onOpen: () => console.log('Connected to WebSocket'),
		onMessage: (event) => console.log('New message received:', event.data),
		onClose: () => console.log('Disconnected from WebSocket'),
	});

	const [inputValue, setInputValue] = useState('');

	const handleSend = () => {
		sendMessage(inputValue);
		setInputValue('');
	};

	return (
		<div>
			<h3>WebSocket Chat</h3>
			<div>
				<p>{isConnected ? 'Connected' : 'Disconnected'}</p>
				<input type="text" value={inputValue} onChange={(e) => setInputValue(e.target.value)} placeholder="Type your message..." />
				<button onClick={handleSend} disabled={!isConnected}>
					Send
				</button>
			</div>
			<div>
				<h4>Last Message:</h4>
				<p>{lastMessage}</p>
			</div>
		</div>
	);
}
```

How It Works
Connection Management: The hook automatically manages the WebSocket connection and allows reconnecting after disconnection.
Sending Messages: You can send messages through the sendMessage function, which only sends when the WebSocket is open.
Event Handlers: Custom event handlers (like onMessage) let you react to WebSocket events without polluting your component code.
Last Message: You can access the last received message directly, simplifying data access in the UI.

### useInfiniteScroll

--- The useInfiniteScroll hook is useful for fetching data as a user scrolls down, a pattern commonly seen on social media feeds or in search results. It can handle complex states like loading, error handling, and pagination.

```javascript
import { useState, useEffect, useRef, useCallback } from 'react';

function useInfiniteScroll(fetchData, options = {}) {
	const { threshold = 0.8, hasMore = true } = options;
	const [data, setData] = useState([]);
	const [loading, setLoading] = useState(false);
	const [error, setError] = useState(null);
	const observerRef = useRef();

	const loadMore = useCallback(async () => {
		if (loading || !hasMore) return;
		setLoading(true);
		setError(null);
		try {
			const newData = await fetchData();
			setData((prevData) => [...prevData, ...newData]);
		} catch (err) {
			setError(err);
		} finally {
			setLoading(false);
		}
	}, [fetchData, loading, hasMore]);

	useEffect(() => {
		if (!hasMore) return;

		const observer = new IntersectionObserver(
			(entries) => {
				if (entries[0].isIntersecting) {
					loadMore();
				}
			},
			{ threshold },
		);

		if (observerRef.current) observer.observe(observerRef.current);

		return () => {
			if (observerRef.current) observer.unobserve(observerRef.current);
		};
	}, [loadMore, hasMore, threshold]);

	return { data, loading, error, observerRef };
}

// Usage:
function InfiniteScrollList() {
	const fetchMoreData = async () => {
		const response = await fetch('/api/data'); // Example API
		return response.json();
	};

	const { data, loading, error, observerRef } = useInfiniteScroll(fetchMoreData, {
		threshold: 0.9,
		hasMore: true,
	});

	return (
		<div>
			{data.map((item, index) => (
				<div key={index}>{item}</div>
			))}
			<div ref={observerRef} style={{ height: '1px' }} />
			{loading && <p>Loading...</p>}
			{error && <p>Error loading data...</p>}
		</div>
	);
}
```

### useThrottle

--- The useThrottle hook allows you to throttle function calls, limiting how frequently they are executed. This can be useful for events like resizing, scrolling, or form input changes.

```javascript
import { useRef } from 'react';

function useThrottle(callback, delay) {
	const lastCall = useRef(0);

	return (...args) => {
		const now = new Date().getTime();

		if (now - lastCall.current >= delay) {
			lastCall.current = now;
			callback(...args);
		}
	};
}

// Usage
function ThrottledScroll() {
	const handleScroll = useThrottle(() => {
		console.log('Scrolled!');
	}, 200);

	useEffect(() => {
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	}, [handleScroll]);

	return <div style={{ height: '2000px' }}>Scroll down to see throttled console logs.</div>;
}
```
