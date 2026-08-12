# WAHA Key Endpoints for Channels & Messages

Base: `http://localhost:3000` (or your host). Session usually `default`.

## Channels
- List channels: `GET /api/{session}/channels?role=OWNER|ADMIN|SUBSCRIBER`
- Get channel: `GET /api/{session}/channels/{channelId}`
- Create channel: `POST /api/{session}/channels`
- Preview messages (public, by invite): `GET /api/{session}/channels/{invite}/messages/preview?limit=100&downloadMedia=false`
- Get messages (subscribed/owned): `GET /api/{session}/chats/{channelId}/messages`  (channelId ends with `@newsletter`)

## Messages with Time Bounds
```
GET /api/{session}/chats/{chatId}/messages
  ?limit=100
  &offset=0
  &downloadMedia=false
  &filter.timestamp.gte=1727745026   # Unix seconds, inclusive
  &filter.timestamp.lte=1727831426
  &filter.fromMe=false
```

Convert ISO dates to Unix with standard tools or:
```js
Math.floor(new Date('2026-08-01T00:00:00Z').getTime() / 1000)
```

## Send to Channel
```
POST /api/sendText
{
  "session": "default",
  "chatId": "123456789012345678@newsletter",
  "text": "Your message here"
}
```

Media, polls, reactions have dedicated endpoints under `/api/...` or channel-specific paths. See official WAHA docs for full list.

## Webhooks
Configure once for real-time aggregation (event: `message`). Useful for continuous monitoring instead of polling.
