### **HTTP Requests with Async/Await**

---

### **Objective:**

- Learn how HTTP requests work
- Use `fetch()` with `async/await`
- Handle errors in HTTP requests
- Make multiple requests efficiently
- Build a small project using a public API

---

## **1. What is an HTTP Request?**

When you visit a website or fetch data from an API, you're making an **HTTP request**.

There are different request methods:

- **GET** → Retrieve data
- **POST** → Send data
- **PUT/PATCH** → Update data
- **DELETE** → Remove data

We’ll focus on **GET requests** using the [Pokémon API](https://pokeapi.co/) and [Star Wars API](https://swapi.dev/).

---

## **2. Fetching Data with Async/Await**

JavaScript provides `fetch()` to make HTTP requests.

### **Example: Fetch a Pokémon**

```javascript
async function getPokemon(name) {
  try {
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${name}`);
    if (!response.ok) throw new Error("Pokémon not found");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error.message);
  }
}

getPokemon("pikachu");
```

### **Explanation:**

1. `fetch()` makes a request to the Pokémon API.
2. `await` waits for the response.
3. `.json()` converts the response to an object.
4. We handle errors if the Pokémon doesn't exist.

---

## **3. Making Multiple Requests**

Sometimes, you need data from multiple endpoints.

### **Example: Fetch Two Pokémon**

```javascript
async function getTwoPokemon(pokemon1, pokemon2) {
  try {
    let [poke1, poke2] = await Promise.all([
      fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon1}`).then((res) =>
        res.json()
      ),
      fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon2}`).then((res) =>
        res.json()
      ),
    ]);

    console.log(poke1.name, poke2.name);
  } catch (error) {
    console.error("Error fetching Pokémon:", error);
  }
}

getTwoPokemon("bulbasaur", "charmander");
```

### **Explanation:**

1. `Promise.all()` runs both requests **at the same time**.
2. `.json()` converts both responses to objects.
3. We log the Pokémon names.

---

## **4. Error Handling in API Calls**

APIs may fail due to bad requests, server issues, or no internet.

### **Example: Handling Errors**

```javascript
async function fetchWithErrorHandling(pokemon) {
  try {
    let response = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon}`);

    if (!response.ok) throw new Error(`Error: ${response.status}`);

    let data = await response.json();
    console.log(data.name);
  } catch (error) {
    console.error("Request failed:", error.message);
  }
}

fetchWithErrorHandling("invalidPokemon");
```

---

## **5. Project: Pokémon Info Card**

### **Goal:**

- Input a Pokémon name
- Fetch its image and details
- Display a simple card

### **HTML**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Pokémon Card</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        text-align: center;
      }
      .card {
        display: inline-block;
        padding: 20px;
        border: 1px solid #000;
        border-radius: 10px;
        width: 200px;
        margin-top: 20px;
      }
      img {
        width: 100px;
      }
    </style>
  </head>
  <body>
    <h2>Pokémon Info</h2>
    <input type="text" id="pokemonName" placeholder="Enter Pokémon name" />
    <button onclick="fetchPokemon()">Search</button>
    <div id="pokemonCard"></div>

    <script>
      async function fetchPokemon() {
        let name = document.getElementById("pokemonName").value.toLowerCase();
        let card = document.getElementById("pokemonCard");

        try {
          let response = await fetch(
            `https://pokeapi.co/api/v2/pokemon/${name}`
          );
          if (!response.ok) throw new Error("Pokémon not found");

          let data = await response.json();

          card.innerHTML = `
                    <div class="card">
                        <h3>${data.name.toUpperCase()}</h3>
                        <img src="${data.sprites.front_default}" alt="${
            data.name
          }">
                        <p>Type: ${data.types
                          .map((t) => t.type.name)
                          .join(", ")}</p>
                        <p>Height: ${data.height}</p>
                        <p>Weight: ${data.weight}</p>
                    </div>
                `;
        } catch (error) {
          card.innerHTML = `<p style="color:red;">${error.message}</p>`;
        }
      }
    </script>
  </body>
</html>
```

### **How It Works:**

1. User types a Pokémon name.
2. Clicks the "Search" button.
3. Fetches data from the API.
4. Displays an image, type, height, and weight.

---

## **Exercises**

### **1. Fetch Star Wars Character**

Modify the Pokémon example to fetch a Star Wars character from `https://swapi.dev/api/people/`.

Example:

```javascript
async function getCharacter(name) {
  let response = await fetch(`https://swapi.dev/api/people/?search=${name}`);
  let data = await response.json();
  console.log(data.results[0]);
}
getCharacter("Luke Skywalker");
```

### **2. Fetch Pokémon Abilities**

Modify the Pokémon project to display its **abilities**.

Hint:

```javascript
data.abilities.map((a) => a.ability.name).join(", ");
```

### **3. Fetch and Display Pokémon Evolution**

Find the Pokémon's evolution chain using this endpoint:

```
https://pokeapi.co/api/v2/evolution-chain/{id}/
```

---

## **Wrap-Up**

- You learned to fetch API data with `async/await`.
- You handled errors properly.
- You made multiple requests efficiently.
- You built a small Pokémon info card.

Try modifying the project and experiment with different APIs!
