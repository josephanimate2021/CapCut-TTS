# What is this
Self-hosted RapperAPI for CapCut's reading API

# NEWS
### 2024/05/29
> **A bug where tokens could not be obtained has been fixed! **

### 2024/05/27
> **I found a clue to a new method! ! I will update the fix soon! ! **

### 2024/02/07
> **As of 2024/02/07, the API version has changed and it is no longer possible to generate a token using the newly acquired `SIGN` and `DEVICE_TIME`. ** <br>
We are currently preparing a new way to obtain a token.

# How To Use
## Create .env file
Create a `.env` file by copying it from the `.env.example` file and check if the following items are in it.
```
DEVICE_TIME=""
SIGN=""
```
Please enter the values ​​obtained in the following steps into each item.

## Get value
![1](/images/1.png)

After logging in to CapCut, create a new empty project and create an appropriate text.

After that, go to the text-to-speech tab and click `Clear Network log' in DevTools to clear the log and make it easier to see.

![2](/images/2.png)

Then, select an appropriate voice and generate it.

Then, some communication will occur and a log will appear.

If you filter the first `2000ms` and look at it, you should see a communication log like the image below. (If there are many, search for `token` from `Filter`.)

Among them, find the `token` that is communicating with `POST`.

Copy the two values ​​in the image, `Device-Time` and `Sign`, from the request header of that communication and enter them into each variable in the `.env` file you created earlier.

# API Docs
## Base URL
```
http://<host>:<port>/v1/
```
## end point
### synthesize
#### request
```http
GET /v1/synthesize
```
#### Clier parameters
| Parameter | Type | Required | Description | Default value |
|--------------|----|------|------|--------------|
| `text` | string | Yes | Text to read | - |
| `type` | number | No | Type of synthetic voice to use | `0` |
| `pitch` | number | No | Pitch of synthesized voice | `10` |
| `speed` | number | No | Speed ​​of synthesized voice | `10` |
| `volume` | number | No | Volume of synthesized audio | `10` |
| `method` | string | No | Method to use for synthesized speech response<br>Either `buffer` or `stream` | `buffer` |

#### Response
| Status code | Condition | Description | Content |
|------------------|------|------|------|
| `200 OK` | Success | Returns the synthesized audio in WAV format | For the `buffer` method: Returns the audio as a complete buffer<br>For the `stream` method: Streams the audio data Masu |
| `400 Bad Request` | Error | Missing query parameters | ```json { "error": "Bad Request" } ``` |
| `500 Internal Server Error` | Error | Problem generating audio | For `buffer` method: ```json { "error": "can't get buffer" } ``` <br>` For the stream` method: ```json { "error": "can't get stream" } ``` |

#### Request example
```http
GET http://localhost:8080/v1/synthesize?text=Hello&type=0&pitch=10&speed=10&volume=10&method=buffer
```

#### Response example
| Status Code | Content-Type | Content |
|------------------|--------------|------|
| `200 OK` | audio/wav | WAV format audio buffer |

## Voice Type List
| type | Voice type | Speaker ID |
|------|------------------|-------------------------|
| 0 | Mystery 1 Boys 1 | BV525_streaming |
| 1 | Mystery 2 Boy | BV528_streaming |
| 2 | Kawabo | BV017_streaming |
| 3 | Big sister | BV016_streaming |
| 4 | Girls | BV023_streaming |
| 5 | Women | BV024_streaming |
| 6 | Men 2 | BV018_streaming |
| 7 | Botchan | BV523_streaming |
| 8 | Women | BV521_streaming |
| 9 | Female announcer | BV522_streaming |
| 10 | Male announcer | BV524_streaming |
| 11 | Genki Loli | BV520_streaming |
| 12 | Bright Honey | VOV401_bytesing3_kangkangwuqu |
| 13 | Kind lady | VOV402_bytesing3_oh |
| 14 | 风雅メゾソプラノ | VOV402_bytesing3_aidelizan |
| 15 | Sakura | jp_005 |
| Other/No input | Older sister | BV016_streaming |

