# Outputs
Outputs allow you to send an audio or video stream elsewhere, such as to YouTube, or writing to a file. There can be any number of outputs, and tcan be created, updated, and deleted using the [API](api.md). They can also be created at start-up using a [config file](config_file.md).

## Common properties
All outputs have the following properties:

| Name | Can be set initially? | Can be updated?? | Description | Default value (if not set) |
| ---- | --------------------- | ---------------- | ----------- | -------------------------- |
| `id` | No | No | ID of the output. Positive integer. Starts at 1 and increases by 1 for each new output. | n/a  |
| `uid` | No | No | Unqiue ID - a string in the format 'outputX' where X is the ID | n/a  |
| `type` | Yes | No | The output type, e.g. `rtmp`. | N/A - *REQUIRED* |
| `state` | Yes | Yes | Either `NULL`, `READY`, `PAUSED` or `PLAYING`. [_What are the four states?_](faq.md#what-are-the-four-states) | `PLAYING` |
| `desired_state` | No (Use `state`) | No (Use `state`) | Set to state that the user has requested, when it has not yet been reached. |
| `source` | Yes | Yes, but only if the output is in the `READY` or `NULL` states. | The source of the output - either an [input](inputs.md), or a [mixer](mixers.md), or `null`. | None (`null`) |

## Output types
Brave currently support these output types:

- [rtmp](#rtmp)
- [tcp](#tcp)
- [file](#uri)
- [image](#image)
- [webrtc](#webrtc)
- [kvs](#kvs)
- [local](#local)


### rtmp
The `rtmp` output forwards the audio/video to an RTMP server. This can include YouTube, Facebook Live and Periscope.

### Properties
In addition to the common properties above, this input type also has the following:

| Name | Can be set initially? | Can be updated?? | Description | Default value (if not set) |
| ---- | --------------------- | ---------------- | ----------- | -------------------------- |
| `uri` | Yes | The URI of the content | n/a (required) |
| `width` and `height` | No | Width and height of video | Whatever the source is |


### tcp
The `tcp` output acts as  TCP server, allowing a client to connect and retrieve the content over the TCP protocol. VLC is able to connect in this way.

### Properties
In addition to the common properties above, this output type also has the following:

| Name | Can be set initially? | Can be updated?? | Description | Default value (if not set) |
| ---- | --------------------- | ---------------- | ----------- | -------------------------- |
| `host` | Yes | No | The name of the host to assume | The server's hostname |
| `port` | Yes | No | The port to run the TCP server on | An available port |
| `width` and `height` | Yes | No | Width and height of video | Whatever the source is |
| `audio_bitrate` | Yes | No | The audio bitrate to use. | 128,000 bps |
| `container` | Yes | No | The video container to use - either `mpeg` or `ogg` | `mpeg` |

### file
The `file` output allows the content to be written to a file. At present the file will always be encoded with h264 for video and AAC for audio, ideal for an MP4 file.

### Properties
In addition to the common properties above, this output type also has the following:

| Name | Can be set initially? | Can be updated?? | Description | Default value (if not set) |
| ---- | --------------------- | ---------------- | ----------- | -------------------------- |
| `location` | Yes | No | The location of the file to write to | n/a (required) |
| `width` and `height` | Yes | No Width and height of video | Whatever the source is |

### image
The `image` output writes a snapshot image of the video periodically. The image can then be accessed periodically through the API call `/api/outputs/1/body`.

### Properties
In addition to the common properties above, this output type also has the following:

| Name | Can be set initially? | Can be updated?? | Description | Default value (if not set) |
| ---- | --------------------- | ---------------- | ----------- | -------------------------- |
| `width` and `height` | Yes | No | Width and height of video | 640x360 |

### webrtc
The `webrtc` output allows the audio/video to be previewed via WebRTC, which works well in a modern web browser.

Once created, clients must instantiate the connection via websocket.

### kvs
The `kvs` output sends video (not audio) to the [Kinesis Video Stream](https://aws.amazon.com/kinesis/video-streams/) service that's part of AWS.

See the specific [installation instructions](docs/install_kvs.md) for installing the relevant GStreamer element.

Note: the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables must be set, in the same way that they are needed for the [AWS CLI[(https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html).

### Properties
In addition to the common properties above, this output type also has the following:

| Name | Can be set initially? | Can be updated?? | Description | Default value (if not set) |
| ---- | --------------------- | ---------------- | ----------- | -------------------------- |
| `stream_name` | Yes | No | Name of the Kinesis Video stream. | n/a |
| `width` and `height` | Yes | No | Width and height of video | 640x360 |


### local
The `local` output creates a Gtk window to show the video. This only works when Brave is running on a local machine. (Brave is designed to work remotely, and so this output is assumed to be needed only for debugging.)
