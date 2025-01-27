# 🚀 Tricks

**🌐 A list tricks 🌐**

### 🔴 Delete navigation item in GPT UI
```javascript
const token = 'eyJhbGciOiJSUzI1NiIsImtpZCI6IjE5MzQ0ZTY1LWJiYzktNDRkMS1hOWQwLWY5NTdiMDc5YmQwZSIsInR5cCI6IkpXVCJ9.eyJhdWQiOlsiaHR0cHM6Ly9hcGkub3BlbmFpLmNvbS92MSJdLCJjbGllbnRfaWQiOiJhcHBfWDh6WTZ2VzJwUTl0UjNkRTduSzFqTDVnSCIsImV4cCI6MTczODc3Mjg1MywiaHR0cHM6Ly9hcGkub3BlbmFpLmNvbS9hdXRoIjp7InVzZXJfaWQiOiJ1c2VyLW9hYVhMZG5KQjhoRDBIN1RScUV3NDFidSJ9LCJodHRwczovL2FwaS5vcGVuYWkuY29tL3Byb2ZpbGUiOnsiZW1haWwiOiJuZ2hpYWt5ZGllbUBnbWFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZX0sImlhdCI6MTczNzkwODg1MiwiaXNzIjoiaHR0cHM6Ly9hdXRoLm9wZW5haS5jb20iLCJqdGkiOiJhMjc5ZWVjOC04YTRjLTQxMzYtYjY5NS1iZDJkMjk5ZmUwMmEiLCJuYmYiOjE3Mzc5MDg4NTIsInB3ZF9hdXRoX3RpbWUiOjE3Mzc5MDg4NTA3MjksInNjcCI6WyJvcGVuaWQiLCJlbWFpbCIsInByb2ZpbGUiLCJvZmZsaW5lX2FjY2VzcyIsIm1vZGVsLnJlcXVlc3QiLCJtb2RlbC5yZWFkIiwib3JnYW5pemF0aW9uLnJlYWQiLCJvcmdhbml6YXRpb24ud3JpdGUiXSwic2Vzc2lvbl9pZCI6ImF1dGhzZXNzX2diUFNSVDE2dEJZRUdvZWs2N2JkMTJBbSIsInN1YiI6Imdvb2dsZS1vYXV0aDJ8MTAzNTkwNjIxNjE4OTg3NTI4NTM2In0.KUe8bfV9jmlxRoIK9dCMZjRkD5hZa7-hj9CyNgS-p6ukfOmVJ-fySggK21kHl2VvbuXa5KS5qA4SidIh017QY6TUvBhIkIAfxOPH4blqa3Ozh5vV2DZjkh7BIz79KVcnPSwBJshdj3xLkvgJzv0l4-lBZkkTp4E0y3L7eZWcm0GkKUL1e4gYiz9qZ13qA1TNWjMmkHFfqoYC6LlWXzBrRq_Y7a_WX1s75fWuqwec_tVuihfidgdi7Ua9gqu6kHKK5Iq6Ol4-A1eRKNNrAWBzmIIdX7Dg04M8RNXTyXrpK6zS3fDESbqtpmKMPd75JzGs49I7bfO1YLnlEG4_bxhc2xD21FomvBLtNa5DvNo0XPbjN9CuSP3vTNKu8aZqtzPeHlh18_DfwJUTzTpOfFi4T-0KlYj32VUJvz89V1ZCfauo3GE6WSN1IAcrSDeRHaO3y_ZO5gsWJPIBy-knq2C5xudvXsuWMj0T9MsF2uvkCtVeqZxTwOqYItHIpOnwRzvDI02lzik8cazkKAjYWSpmSMC9yIVA31SlLHy734S4zjvIW3sMiDk2YlHpFj9HV8rIgUf1G6RQ1C_NZo8u9DNbG3dNfp_wowOKRt0VtNRnZBpce3d3u2qTaWIVc0JTXwL0_jHOe7jF1Seh9QIobosuj5DZ3ZNPjGsC-2Fk65t7FiQ';
const arrIndexToDelete = [0];
const elements = document.querySelectorAll('li > div > a[data-discover="true"]');

arrIndexToDelete?.forEach((id) => {
  const element = elements[id] || null;
  
  if (!element) {
    console.log(`Element does not exist at index ${id}.`);
    return;
  }

  const parts = element?.href?.split('/');
  const elementId = parts?.length ? parts[parts.length - 1] : null;

  if (!elementId) {
    console.log(`Element at index ${id} does not have a valid URL.`);
    return;
  }

  // Send the PATCH request
  fetch(`https://chatgpt.com/backend-api/conversation/${elementId}`, {
    method: 'PATCH',
    headers: {
      accept: '*/*',
      'accept-language': 'en-US,en;q=0.9,vi;q=0.8',
      authorization: `Bearer ${token}`,
      'content-type': 'application/json',
    },
    body: JSON.stringify({
      is_visible: false,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      console.log('Request was successful:', data);
    })
    .catch((error) => {
      console.error('Error making the request:', error);
    });
});
```