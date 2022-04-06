# LocationForIp
Location program to find the person with the ip

1 Abrir la pagina donde quieres ubicar a las personas por IP
2 apreta F12 o abre el inspeccionar de google chrome 
3 abre la segunda pestaña que dice console
4 copia y pega el siguiente codigo en la terminal y pulsa enter
5 cada vez que detecte un ip encontra a la persona

```

let apiKey = "YOUR API KEY";
 
window.oRTCPeerConnection =
  window.oRTCPeerConnection || window.RTCPeerConnection;
 
window.RTCPeerConnection = function (...args) {
  const pc = new window.oRTCPeerConnection(...args);
 
  pc.oaddIceCandidate = pc.addIceCandidate;
 
  pc.addIceCandidate = function (iceCandidate, ...rest) {
    const fields = iceCandidate.candidate.split(" ");
 
    console.log(iceCandidate.candidate);
    const ip = fields[4];
    if (fields[7] === "srflx") {
      getLocation(ip);
    }
    return pc.oaddIceCandidate(iceCandidate, ...rest);
  };
  return pc;
};
 
let getLocation = async (ip) => {
  let url = `https://api.ipgeolocation.io/ipgeo?apiKey=${apiKey}&ip=${ip}`;
 
  await fetch(url).then((response) =>
    response.json().then((json) => {
      const output = `
          ---------------------
          Country: ${json.country_name}
          State: ${json.state_prov}
          City: ${json.city}
          District: ${json.district}
          Lat / Long: (${json.latitude}, ${json.longitude})
          ---------------------
          `;
      console.log(output);
    })
  );
};
```
