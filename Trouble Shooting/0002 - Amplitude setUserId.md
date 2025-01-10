* deprecated
```javascript
window.amplitude.add(window.sessionReplay.plugin({ sampleRate: 1 }));  
window.amplitude.init('key', { 'autocapture': { 'elementInteractions': true } });  
  
fetch('https://backend_url/api/v1/users/me', {  
  credentials: 'include',  
}).then((res) => {  
  return res.json();  
}).then((body) => {  
  const userId = 'USER-00000000000' + body.result.id;  
  window.amplitude.setUserId(userId);  
});
```

