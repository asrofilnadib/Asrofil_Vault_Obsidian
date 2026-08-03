---
defines-react-components: true
---

```jsx:component:ShowI

const files = app.vault.getMarkdownFiles();
let count = 0;
const cells = [];

for (const file of files) {
  const today = new Date();
  const year = today.getFullYear().toString();
  const basename = file.basename;
  const meta = app.metadataCache.getFileCache(file)?.frontmatter;

  if (!meta?.birthday) continue;

  const birthday = meta.birthday;
  const nextBirthdayString = year + birthday.slice(4);
  const nextBirthday = new Date(nextBirthdayString);
  let days = Math.floor((nextBirthday - today) / 86400000);
  if (days < 0) days += 365;

  if (days < 31) {
    count++;
    cells.push(
      <div className="birdcountdown">
        <text className="birdname">{basename}'s Birthday</text>
        <text className="countdowndays"> {days + 1} days</text>
      </div>
    );
  }
}

const element = <div>{count} birthdays in the next month:<br /></div>;

const iframeStyle = {
  frameBorder: "no",
  border: "0",
  marginWidth: "0",
  marginHeight: "0",
  width: 330,
  height: 110,
};

return (
  <div className="musicBirthday">
    <div style={{ flex: 1, textAlign: "center", whiteSpace: "pre-wrap" }}>
      <p><b>MUSIC OF THE MONTH</b></p>
      <p style={{ fontSize: "0.9em", opacity: 0.8 }}>
        Tambah playlist embed di file ini
      </p>
    </div>
    <div style={{ flex: 1, textAlign: "center", whiteSpace: "pre-wrap" }}>
      <p><b>UPCOMING BIRTHDAYS</b></p>
      <p>{element}{cells}</p>
    </div>
  </div>
);
```

```jsx:
<ShowI></ShowI>
```
