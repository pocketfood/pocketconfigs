# 📁 Basic Guide: HTTP File Upload via XMPP (Using Gajim + curl)

## 1. Request an HTTP Upload Slot (using for example Gajim XML Console)

To upload a file, you need to request a slot from the server.

1. Open Gajim
2. Go to **Tools → XML Console**
3. Send the following stanza (replace values appropriately):

```xml
<iq from='user@example.tld/resource'
    id='slot_01'
    to='upload.example.tld'
    type='get'>
  <request xmlns='urn:xmpp:http:upload:0'
           filename='example.jpg'
           size='23456'
           content-type='image/jpeg' />
</iq>
```

- `filename`: name with extension (e.g., `example.jpg`)
- `size`: file size in bytes
- `content-type`: MIME type

---

## 2. Receive Slot Response

You should get a response like this:

```xml
<iq type='result'
    id='slot_01'
    to='user@example.tld/resource'>
  <slot xmlns='urn:xmpp:http:upload:0'>
    <put url='https://upload.example.tld/upload/abc123'/>
    <get url='https://upload.example.tld/upload/abc123/example.jpg'/>
  </slot>
</iq>
```

- **`put` URL**: Use this with `curl` to upload the file
- **`get` URL**: This is the link to share with others

---

## 3. Upload the File Using `curl`

```bash
curl -X PUT -T example.jpg \
  -H 'Content-Type: image/jpeg' \
  'https://upload.example.tld/upload/abc123'
```

> Make sure `example.jpg` is in your current directory.

---

## 4. Share the File

Send the **`get` URL** in your chat:

```
Hey, check out this image: https://upload.example.tld/upload/abc123/example.jpg
```

---

✅ That’s it! You’ve successfully uploaded and shared a file over XMPP using HTTP Upload.
