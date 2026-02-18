{% include nav.html %}

<html>
<head>
  <meta charset="UTF-8">
  <title>Estill County Government Officials</title>
  <style>
    body { font-family: Arial, sans-serif; }
    .official { border: 1px solid #ccc; padding: 10px; margin: 10px 0; display: flex; align-items: center; }
    .official img { width: 100px; height: auto; margin-right: 15px; border-radius: 6px; }
    .official-details { line-height: 1.5; }
  </style>
</head>
<body>

<h1>Estill County Government Officials</h1>
<div id="officials"></div>

<script>
  const officials = [
    {
      name: "Donnie Watson",
      title: "County Judge-Executive",
      phone: "(606) 723-7524",
      website: "https://estillky.com/", // official Estill County website :contentReference[oaicite:1]{index=1}
      imgSrc: "[images/donniewatson.jpg](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ3H1zusmanJX2skhLJYkFIGKZCiD1syyOnww&s)"
    },
    {
      name: "Christopher Flynn",
      title: "County Sheriff",
      phone: "(606) 723-2323",
      website: "https://estillky.com/sheriff/", // official Sheriff page :contentReference[oaicite:2]{index=2}
      imgSrc: "[images/chrisflynn.jpg](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQtXaaxRV8ezmbsHVNPXUmkJJQ-zYCZfzahnQ&s)"
    },
    {
      name: "Kimberly Charles",
      title: "County Clerk",
      phone: "(606) 723-5156",
      website: "https://estill.countyclerk.us/", // County Clerk official site :contentReference[oaicite:3]{index=3}
      imgSrc: "images/kimberlycharles.jpg"
    },
    {
      name: "Stephanie Brinegar-Cassidy",
      title: "Circuit Court Clerk",
      phone: "(606) 723-3970",
      website: "https://kycourts.gov/Courts/County-Information/Pages/Estill.aspx", // KY Courts directory :contentReference[oaicite:4]{index=4}
      imgSrc: "images/stephaniecassidy.jpg"
    },
    {
      name: "Jeff L. Hix",
      title: "Property Valuation Administrator",
      phone: "(606) 723-4569",
      website: "http://www.estillkypva.org/", // typical PVA website :contentReference[oaicite:5]{index=5}
      imgSrc: "images/jeffhix.jpg"
    },
    {
      name: "Jason Riley",
      title: "County Attorney",
      phone: "(606) 723-6262",
      website: "https://ag.ky.gov/Pages/attorneys.aspx", // KY Attorney info site :contentReference[oaicite:6]{index=6}
      imgSrc: "images/jasonriley.jpg"
    },
    {
      name: "Beverly Morris",
      title: "County Jailer",
      phone: "(606) 643-5199",
      website: "https://estillky.com/estill-county-government-contacts/", // general contact list page :contentReference[oaicite:7]{index=7}
      imgSrc: "images/beverlymorris.jpg"
    },
    {
      name: "Tony Murphy",
      title: "County Coroner",
      phone: "(606) 723-9508",
      website: "https://estillky.com/estill-county-government-contacts/", // general contacts :contentReference[oaicite:8]{index=8}
      imgSrc: "images/tonymurphy.jpg"
    }
  ];

  const container = document.getElementById("officials");

  officials.forEach(off => {
    container.innerHTML += `
      <div class="official">
        <img src="${off.imgSrc}" alt="Photo of ${off.name}">
        <div class="official-details">
          <strong>${off.name}</strong><br>
          <em>${off.title}</em><br>
          Phone: <a href="tel:${off.phone}">${off.phone}</a><br>
          Website: <a href="${off.website}" target="_blank">${off.website}</a>
        </div>
      </div>
    `;
  });
</script>

</body>
</html>
