/home/user/nikk-tires/README.md
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <title>Reifenservice – Terminbuchung</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #111;
      color: #eee;
      margin: 0;
      padding: 0;
    }
    header {
      background: #0f0;
      color: #111;
      padding: 1rem;
      text-align: center;
      font-weight: bold;
    }
    main {
      max-width: 600px;
      margin: 2rem auto;
      padding: 2rem;
      background: #1c1c1c;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,255,0,0.3);
    }
    h2 { color: #0f0; }
    label { display: block; margin: 1rem 0 0.5rem; }
    input, select, textarea {
      width: 100%;
      padding: 0.5rem;
      border: 1px solid #0f0;
      border-radius: 4px;
      background: #222;
      color: #eee;
    }
    button {
      margin-top: 1.5rem;
      padding: 0.7rem 1.5rem;
      background: #0f0;
      color: #111;
      font-weight: bold;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover { background: #0c0; }
    .message {
      margin-top: 1rem;
      font-weight: bold;
    }
    .success { color: #0f0; }
    .error { color: #f33; }
  </style>
</head>
<body>
  <header>
    Reifenservice Andrei Nikkel – Terminbuchung
  </header>
  <main>
    <h2>Termin vereinbaren</h2>
    <form id="bookingForm">
      <label for="name">Name*</label>
      <input type="text" id="name" name="name" required>

      <label for="phone">Telefon*</label>
      <input type="tel" id="phone" name="phone" required>

      <label for="email">E-Mail</label>
      <input type="email" id="email" name="email">

      <label for="date">Datum*</label>
      <input type="date" id="date" name="date" required>

      <label for="time">Uhrzeit*</label>
      <input type="time" id="time" name="time" required>

      <label for="type">Service</label>
      <select id="type" name="type">
        <option value="Reifenwechsel">Reifenwechsel</option>
        <option value="Reparatur">Reparatur</option>
        <option value="Check">Reifen-Check</option>
      </select>

      <label for="notes">Notizen</label>
      <textarea id="notes" name="notes" rows="3"></textarea>

      <button type="submit">Termin speichern</button>
    </form>
    <div id="message" class="message"></div>
  </main>

  <script>
    const form = document.getElementById('bookingForm');
    const messageEl = document.getElementById('message');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      messageEl.textContent = "Sende Daten...";
      messageEl.className = "message";

      const data = {
        name: form.name.value,
        phone: form.phone.value,
        email: form.email.value,
        date: form.date.value,
        time: form.time.value,
        type: form.type.value,
        notes: form.notes.value
      };

      try {
        const res = await fetch(
          "https://us-central1-reifencheck-andrei.cloudfunctions.net/createBooking", // Beispiel: deine Projekt-ID
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(data)
          }
        );

        if (!res.ok) {
          throw new Error(await res.text());
        }

        const result = await res.json();
        messageEl.textContent = "✅ Termin erfolgreich gespeichert!";
        messageEl.className = "message success";
        form.reset();
      } catch (err) {
        console.error(err);
        messageEl.textContent = "❌ Fehler: " + err.message;
        messageEl.className = "message error";
      }
    });
  </script>
</body>
</html>
