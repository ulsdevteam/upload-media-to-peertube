# Description:

This script is used to read from a Google Sheet and upload the files
referenced there to a PeerTube instance and then update the PeerTube
object URL back into the Google Sheet.

## Google Sheet requirements:

Sheet Columns:

  ---------------------------------------------------------------------------
  **Required Columns**         
  ---------------------------- ----------------------------------------------
  **Google Sheet**             **PeerTube**

  'id'                         This is the PID of the object. This will be
                               appended to the description field as '({id})'.

  'title'                      This is the title displayed of the video. It
                               will be truncated to 250 characters within
                               PeerTube.

  'description'                Description -- A description of the media.
                               This can be empty.

  'language'                   Language -- This can be empty, use empty for
                               silent (no audio) media. Eg. English, Spanish,
                               French, etc.

  'file'                       Media File that is uploaded (.mp3\|.mkv) --
                               Must be the full path to the media file.
                               Usually populated by scan-batch-dir script.

  'field_media_oembed_video'   Leave this empty -- This field will be updated
                               by the script with the URL of the PeerTube
                               object.

  **Optional Columns**         

  **Google Sheet**             **PeerTube**

  'thumbnail'                  Thumbnail file for the media. This would be
                               the full path to the Thumbnail (.jpg or .png).
                               Usually populated by scan-batch-dir script.

  'transcript'                 Transcript file for the media. This is the
                               full path to either a .srt (SubRip) or .vtt
                               (WebVTT) format file. Usually populated by
                               scan-batch-dir script.

  'transcript_language'        If a transcript is provided, this is the
                               language of the transcript file (English,
                               Spanish, French, etc.). Usually required if a
                               transcript file is added.
  ---------------------------------------------------------------------------

## Script Parameters:

  ------------------------------------------------------------------------
  Parameter                 Description
  ------------------------- ----------------------------------------------
  \--config-file            Path to the script config file. Contains the
                            username, password, instance, and Channel for
                            connecting to Peertube. Contains the path to
                            the google credentials file (.json), Google
                            Sheet ID, and Google Sheet Name (tab) for
                            connecting to a Google Sheet.

  \--log-file               Path to where the generated log file will be
                            created.

  \--in-google-sheet-id     The ID number of the Google Sheet.

  \--in-google-sheet-name   The Name of the Tab in the Google Sheet.
                            (E.g.: Sheet1)

  \--in-google-creds-file   Path to where the Google Credentials File
                            .json file is located.

  \--peertube-instance      The PeerTube instance URL. E.g.
                            <https://media.library.pitt.edu>

  \--peertube-username      The PeerTube account username.

  \--peertube-password      The PeerTube account password.

  \--peertube-channel       The PeerTube channel number.
  ------------------------------------------------------------------------

## Google Credentials File:

This is a .json file provided by ULS IT that contains the information
needed to connect to a Google Drive and Google Sheets via a service
account using an API.

## Google Sheet ID:

This is a long string of characters found in the URL bar when
editing/viewing the Google Sheet in a browser. E.g.:
'2cip4rM-c9Xnc2oisIM_Win5qODMyCl97A7pBJLElUYv'. It is located between
the "https://docs.google.com/spreadsheets/d/" and "/edit..." in the URL
bar. Make sure you choose the correct Google Sheet ID as this ID is used
to identify the Google Sheet the script will update with the PeerTube
URL. In general, each batch of media objects will have its own unique
Google Sheet ID.

## Usage:

Script Usage Example:

upload-media-to-peertube ---config-file config.conf ---log-file
batch_name.log ---in-google-sheet-id {Google Sheet ID -- see above}
---in-google-sheet-name Sheet1

## Function:

For each row in the Google Sheet obtain the required information (see
above). This is the minimum information needed for uploading a media
file to PeerTube. And construct a media object for PeerTube and upload
it using the PeerTube API to the user and channel indicated by the
entries in the configuration file.

If there is a thumbnail file at the time of upload, it will add the
thumbnail to the media file upon upload to PeerTube. If not, a default
or no thumbnail will be provided. Optionally, a frame from the media can
be used to manually set a thumbnail for the media.

Additionally, if there is data in both a transcript (WebVTT format) and
transcript_language (English, Chinese, etc.) columns in the Google Sheet
the script will additionally add the transcript file as a caption file
to the just uploaded media file object and this will be used as a
transcript/caption for the media in PeerTube.

PeerTube Accepted Languages List:

<https://docs.google.com/spreadsheets/d/1IcFWZtHaBPb3-i8DVyWxVv9VVJCMQKfizWc6k3wSdDA/>

Or: <https://media.library.pitt.edu/api/v1/videos/languages>
