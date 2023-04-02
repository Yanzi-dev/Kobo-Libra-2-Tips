# Kobo-Libra-2-Tips
Kobo Libra 2 Tips

## Disclaimer

Please note that the information provided in this documentation is for educational purposes only and should not be considered as legal or professional advice. While we have made every effort to ensure that the information is accurate and up-to-date, we make no guarantees or warranties of any kind, express or implied, about the completeness, accuracy, reliability, suitability or availability with respect to the information contained in this documentation.

By following the instructions provided in this documentation, you do so at your own risk. We cannot be held liable for any damages or losses that may result from the use of this documentation. It is your responsibility to ensure that you understand and follow all instructions correctly, and to seek professional advice if necessary.

## How to use the Kobo Libra 2 without account

### Wifi ? No

At startup, the e-reader will prompt you to connect to wifi.

At this point, you should select: "Don't have wifi network?"

Then, connect the e-reader to your computer using the USB-C cable.

On the computer, the e-reader will appear as a storage device.

### Sideload

You can enable Sideloaded Mode on Kobo e-readers by adding the following code to the "ApplicationPreferences" section of the `.kobo/Kobo eReader.conf` file: `SideloadedMode=true`.

### Eject your device

When you are done, eject your device from your computer.
Reboot


## Once done, what should I do ?

### Preloaded books.

The e-reader starts up and you may notice that there are already authors and books present.

However, these cannot be accessed.

I chose to delete these entries directly in the SQLite database of the e-reader.

### How to remove Preloaded books

In the .kobo directory, there is a file called KoboReader.sqlite, which is a SQLite database.

The tables of interest are content and ShelfContent:

Take the time to familiarize yourself with the data.

```sql
-- Obtain the distinct ContentId from the ShelfContent table
select distinct ContentId from ShelfContent order by ContentId;

-- Obtain the distinct BookId from the content table
select distinct BookId from content order by bookid;

-- Obtain the content that are in the ShelfContent table
select distinct ShelfContent.ContentId, content.Bookid, content.BookTitle
from content
inner join ShelfContent on content.BookId = ShelfContent.ContentId;
```

### 💥 Danger Zone

Here are the queries to delete the preloaded content.
It will be assumed here that no book has been manually loaded or purchased.

```sql
delete from ShelfContent
where ShelfContent.ContentId in (select content.BookId from content);

delete from content; 
```

Now preloaded books are gone.
