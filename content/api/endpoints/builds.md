## Builds

* [Get a build](#get-a-build)
* [Trigger a build](#trigger-a-build)

### <a name="get-a-build"></a>Get a build

Get the details of a single build.

***
`GET /api/v3/builds/:build`
***

#### Response

```
Status: 200 OK
```

```json
{
  "status": "finished",
  "startedAt": "2015-04-30T17:06:04.686Z",
  "result": "passed",
  "id": "554260f7dc16db44791955a7",
  "url": "https://app.wercker.com/api/v3/builds/554260f7dc16db44791955a7",
  "application": {
    "stack": 5,
    "privacy": "public",
    "createdAt": "2015-01-28T17:04:09.851Z",
    "owner": {
      "meta": {
        "werckerEmployee": false,
        "username": "wercker"
      },
      "userId": "55310d295732ce8a41000054",
      "avatar": {
        "gravatar": "33a5bfbcf8a2b90f40e849b6f1fa5eeb"
      },
      "name": "wercker",
      "type": "wercker"
    },
    "name": "docs",
    "url": "https://app.wercker.com/api/v3/applications/wercker/docs",
    "id": "54c9168980c7075225004157"
  },
  "branch": "hide_api",
  "createdAt": "2015-04-30T17:05:59.209Z",
  "finishedAt": "2015-04-30T17:09:02.753Z",
  "message": "Hide API header",
  "progress": 100
}
```
### <a name="get-a-build"></a>Trigger a new build
Trigger a new build using an `applicationId`. 

Returns a build object.

***
`POST /api/v3/builds/`
***

|Name|Type|Description|
|----|----|-----------|
|projectId|String|**Required.** The projectId for which a build should be triggered.|
|branch|String|**Optional.** The Git branch that the build should use. If not specified, 'master' will be used.|
|commit|String|**Optional.** The Git commit that the build should used. **Requires** `branch` to be set. If not specified, the latest commit is fetched|
|message|String|**Optional.** The message to use for the build. If not specified, the Git commit message is used.|

#### Response
```json
{
  "id": "557591da3f1aefa778000009",
  "url": "http://172.17.8.99/api/v3/builds/557591da3f1aefa778000009",
  "application": {
    "id": "54c9168980c7075225004157",
    "url": "https://app.wercker.com/api/v3/applications/wercker/docs",
    "name": "docs",
    "owner": {
      "type": "wercker",
      "name": "wercker",
      "avatar": {
        "gravatar": "33a5bfbcf8a2b90f40e849b6f1fa5eeb"
      },
      "userId": "55310d295732ce8a41000054",
      "meta": {
        "username": "wercker",
        "werckerEmployee": false
      }
    },
    "createdAt": "2015-06-08T12:47:39.819Z",
    "privacy": "private",
    "stack": 5
  },
  "branch": "master",
  "createdAt": "2015-06-08T13:00:10.180Z",
  "message": "Fixed OAuth error",
  "progress": null,
  "result": "unknown",
  "status": "notstarted"
}%
```