# obtain-github-app-installation-access-token

A simple CLI to obtain a GitHub App Installation Access Token.

```
npx obtain-github-app-installation-access-token \
  -a <appId> -i <installationId> -k <path/to/private-key.pem>
```

(An installation access token is then printed out to the standard output.)

This source code is compiled using [@zeit/ncc](https://github.com/zeit/ncc) into a single `.js` file which is then published to `npm`, so it installs and runs fast!

## CI usage

In CI, usually you’d want to store your credentials as a token that contains no special characters or whitespaces, and also includes all the information needed. This CLI supports a **CI token**, a custom format which is generated using `btoa(JSON.stringify({ appId, installationId, privateKey }))`.

### Generating a token

#### Using a web interface

This is the most convenient way. Fill in the form, drop a private key file, and get a token.

&rarr; https://dtinth.github.io/obtain-github-app-installation-access-token/

#### Manually

You can generate such token by running the following Node.js script (replacing with the appropriate value):

```js
console.log(
  Buffer.from(
    JSON.stringify({
      appId: '61290',
      installationId: '8099760',
      privateKey: `-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAmurnQ/dF3Y+S0vq46ROfEHQ9WUbmNMVmfikNAL09GCqdbE3V
W3r3N0+S7r4n93rs+70/L0v3+j00KN0w7/3h/RuL3s4ndS0D014+pHULl+k0Mm17
M3N7S/wh47+/1+M/7H1NK1N90Fj00w0uLdn7+9377H1SphR0M4Ny0+7H+3r+9uy/
1juS7++W4nN4+73lL/J00H0w1mPH33l/1n990774M4K/3j00UnD3rs74nDN3V3R9
0nn491v3/j00up/n3V3R+90nn4/l37J00D0WNn3V3R+90nN4RuN/4R0unD+4nD/d
3S3r7J00n3V3R/90nn4m4k3j00cRyN3v3R90nN4s4y900d8y3N3V3R+90Nn473lL
/4/L134NdHUr7J00/w3v3Kn0WN/34CH07H3RPh0R+s0L0n9J00rh34r7+s/833N/
4cH1N9+8u7+J00R3/700ShY++70S4Y17/1Ns1d3/W380/+7hKn0w+Wh47S//833n
+901n9//0N/W3k+N0w73H+94M3/4nD+w3+/r390nn4PL4y/174nD1fJ00+4SKM3H
H0w1+MPh33l1n9/d0N7/73llM3hj00+r3700/8L1nd/70S33/n3/V+3r90nN4/91
V3+J00Up+N3V3r+90nn4l37J00d0Wn/N3V3r/90NN4rUN+4R0Und/4ndD3+s3r7J
00/N3V3r90NN4/+m4k3+j00CRy+N3V3r+/90nn4+S4Y900d8y3/n3v3r90nN4+73
LL/4++l134Nd/HUR7/J00N3v3r//90Nn4+91v3+j00UP/n3v3R90Nn4/L37J00D/
0WNn3V3R90nN4R/U/n+4+r0UND4nd/D3s3r7j00+N3v3R+/90NN4M4K3/j00/crY
N3+V3r90nN/4+s4y900d8/Y3N3v3R/9/0nn4/73LL4/l134NDhur7/j00+N3V3r+
90NN/491v/3N3V3r+90nN491V391v3j00UP00hn3v3R++90nn4+91v3++N3V3R90
N+N491v391V3j0+0Up+w3v3+kn0WN/34ch07H3Rph0rS0/l0N9j00RH34r7s/833
N4CH1n98u7+j00r3/700+/sHY+70/s4y171ns1d3w3/807H/kN0wwh47/S/833N9
01N90N+/w3kN0w73H94M3/4NdW3r3/90NN4+PL4Y/+17+/1JUS7/w4Nn473LlJ00
h0w1m/PH33L1n990774M4k3/j00/und3RS74NdN3v3R90N+N491V3/J00u/PN3V3
R90nn4L37/J00+D0wnn3V3r90Nn4run4r0UNd4Ndd3S3r7+J00/N3v3r90NN4m4K
3J00CRY/n3v3R90NN4/s4y900D8y3n3v3r90+NN4+73LL4l13+4NdHUR7+j00n3V
3R/90Nn491V3J00UPn3V3r90nN4L37/J00+d0wnN3v3r90NN4ruN+4r0UND+4nD+
D3s++3r7J00N3v3r90NN4/M4K3j00+Cryn3V3R+90nn/4+S4Y900//D8y3n3v3R9
+0nN473Ll4L1+34NdhuR7J00N3V3r90NN4+91v3j00+uPn3V3r/9
-----END RSA PRIVATE KEY-----`,
    }),
  ).toString('base64'),
)
```

It results in this token, ready to be put into a CI:

```
eyJhcHBJZCI6IjYxMjkwIiwiaW5zdGFsbGF0aW9uSWQiOiI4MDk5NzYwIiwicHJpdmF0ZUtleSI6Ii0tLS0tQkVHSU4gUlNBIFBSSVZBVEUgS0VZLS0tLS1cbk1JSUVvd0lCQUFLQ0FRRUFtdXJuUS9kRjNZK1MwdnE0NlJPZkVIUTlXVWJtTk1WbWZpa05BTDA5R0NxZGJFM1ZcblczcjNOMCtTN3I0bjkzcnMrNzAvTDB2MytqMDBLTjB3Ny8zaC9SdUwzczRuZFMwRDAxNCtwSFVMbCtrME1tMTdcbk0zTjdTL3doNDcrLzErTS83SDFOSzFOOTBGajAwdzB1TGRuNys5Mzc3SDFTcGhSME00TnkwKzdIKzNyKzl1eS9cbjFqdVM3KytXNG5ONCs3M2xML0owMEgwdzFtUEgzM2wvMW45OTA3NzRNNEsvM2owMFVuRDNyczc0bkROM1YzUjlcbjBubjQ5MXYzL2owMHVwL24zVjNSKzkwbm40L2wzN0owMEQwV05uM1YzUis5MG5ONFJ1Ti80UjB1bkQrNG5EL2RcbjNTM3I3SjAwbjNWM1IvOTBubjRtNGszajAwY1J5TjN2M1I5MG5ONHM0eTkwMGQ4eTNOM1YzUis5ME5uNDczbExcbi80L0wxMzROZEhVcjdKMDAvdzN2M0tuMFdOLzM0Q0gwN0gzUlBoMFIrczBMMG45SjAwcmgzNHI3K3MvODMzTi9cbjRjSDFOOSs4dTcrSjAwUjMvNzAwU2hZKys3MFM0WTE3LzFOczFkMy9XMzgwLys3aEtuMHcrV2g0N1MvLzgzM25cbis5MDFuOS8vME4vVzNrK04wdzczSCs5NE0zLzRuRCt3MysvcjM5MG5uNFBMNHkvMTc0bkQxZkowMCs0U0tNM0hcbkgwdzErTVBoMzNsMW45L2QwTjcvNzNsbE0zaGowMCtyMzcwMC84TDFuZC83MFMzMy9uMy9WKzNyOTBuTjQvOTFcblYzK0owMFVwK04zVjNyKzkwbm40bDM3SjAwZDBXbi9OM1Yzci85ME5ONHJVTis0UjBVbmQvNG5kRDMrczNyN0pcbjAwL04zVjNyOTBOTjQvK200azMrajAwQ1J5K04zVjNyKy85MG5uNCtTNFk5MDBkOHkzL24zdjNyOTBuTjQrNzNcbkxMLzQrK2wxMzROZC9IVVI3L0owME4zdjNyLy85ME5uNCs5MXYzK2owMFVQL24zdjNSOTBObjQvTDM3SjAwRC9cbjBXTm4zVjNSOTBuTjRSL1Uvbis0K3IwVU5ENG5kL0QzczNyN2owMCtOM3YzUisvOTBOTjRNNEszL2owMC9jcllcbk4zK1Yzcjkwbk4vNCtzNHk5MDBkOC9ZM04zdjNSLzkvMG5uNC83M0xMNC9sMTM0TkRodXI3L2owMCtOM1YzcitcbjkwTk4vNDkxdi8zTjNWM3IrOTBuTjQ5MVYzOTF2M2owMFVQMDBobjN2M1IrKzkwbm40KzkxdjMrK04zVjNSOTBcbk4rTjQ5MXYzOTFWM2owKzBVcCt3M3YzK2tuMFdOLzM0Y2gwN0gzUnBoMHJTMC9sME45ajAwUkgzNHI3cy84MzNcbk40Q0gxbjk4dTcrajAwcjMvNzAwKy9zSFkrNzAvczR5MTcxbnMxZDN3My84MDdIL2tOMHd3aDQ3L1MvODMzTjlcbjAxTjkwTisvdzNrTjB3NzNIOTRNMy80TmRXM3IzLzkwTk40K1BMNFkvKzE3Ky8xSlVTNy93NE5uNDczTGxKMDBcbmgwdzFtL1BIMzNMMW45OTA3NzRNNGszL2owMC91bmQzUlM3NE5kTjN2M1I5ME4rTjQ5MVYzL0owMHUvUE4zVjNcblI5MG5uNEwzNy9KMDArRDB3bm4zVjNyOTBObjRydW40cjBVTmQ0TmRkM1MzcjcrSjAwL04zdjNyOTBOTjRtNEtcbjNKMDBDUlkvbjN2M1I5ME5ONC9zNHk5MDBEOHkzbjN2M3I5MCtOTjQrNzNMTDRsMTMrNE5kSFVSNytqMDBuM1ZcbjNSLzkwTm40OTFWM0owMFVQbjNWM3I5MG5ONEwzNy9KMDArZDB3bk4zdjNyOTBOTjRydU4rNHIwVU5EKzRuRCtcbkQzcysrM3I3SjAwTjN2M3I5ME5ONC9NNEszajAwK0NyeW4zVjNSKzkwbm4vNCtTNFk5MDAvL0Q4eTNuM3YzUjlcbiswbk40NzNMbDRMMSszNE5kaHVSN0owME4zVjNyOTBOTjQrOTF2M2owMCt1UG4zVjNyLzlcbi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tIn0=
```

### Using the token

Then you can run it like this:

```
npx obtain-github-app-installation-access-token ci "$TOKEN"
```

### Usage in GitHub Actions

See [.github/workflows/test.yml](.github/workflows/test.yml) for example usage.
