
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MKLIONEL MOVIES</title>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: #080a12;
  color: white;
  font-family: Arial, sans-serif;
}

header {
  background: #101522;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 15px;
}

.logo {
  color: #ff1744;
  font-size: 25px;
  font-weight: bold;
}

nav a {
  color: white;
  text-decoration: none;
  margin: 8px;
}

.search {
  padding: 12px;
  border-radius: 20px;
  border: 1px solid #444;
  background: #202637;
  color: white;
  width: 220px;
}

.hero {
  min-height: 350px;
  padding: 75px 30px;
  background:
  linear-gradient(90deg, #080a12, transparent),
  url("https://image.tmdb.org/t/p/original/8b8R8l88Qje9dn9OE8PY05Nxl1X.jpg")
  center/cover;
}

.hero h1 {
  font-size: 45px;
  margin-bottom: 15px;
}

.hero p {
  max-width: 450px;
  line-height: 1.6;
}

button {
  padding: 12px 22px;
  border: none;
  border-radius: 6px;
  background: #ff1744;
  color: white;
  cursor: pointer;
  margin-top: 20px;
}

section {
  padding: 25px 18px;
}

section h2 {
  margin-bottom: 20px;
}

.movies {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(145px, 1fr));
  gap: 18px;
}

.movie {
  background: #151b2a;
  border-radius: 10px;
  overflow: hidden;
  transition: 0.3s;
}

.movie:hover {
  transform: translateY(-6px);
}

.movie img {
  width: 100%;
  height: 220px;
  object-fit: cover;
}

.movie h3 {
  font-size: 15px;
  padding: 10px;
}

.movie p {
  color: #aaa;
  font-size: 12px;
  padding: 0 10px 10px;
}

.movie button {
  margin: 0 10px 12px;
  padding: 9px;
}

footer {
  text-align: center;
  padding: 30px;
  background: #101522;
  color: #aaa;
}
</style>
</head>

<body>

<header>
  <div class="logo">MKLIONEL</div>

  <nav>
    <a href="#">Home</a>
    <a href="#movies">Movies</a>
    <a href="#popular">Popular</a>
  </nav>

  <input class="search" id="search"
  placeholder="Search movies...">
</header>

<div class="hero">
  <h1>WELCOME TO MKLIONEL</h1>
  <p>Discover movies from around the world.
  Explore action, adventure, comedy and more.</p>
  <button onclick="document.getElementById('movies').scrollIntoView()">
    Explore Movies
  </button>
</div>

<section id="movies">
  <h2>🎬 Worldwide Movies</h2>
  <div class="movies" id="movieList"></div>
</section>

<section id="popular">
  <h2>⭐ Popular Movies</h2>
  <p>Discover trending movies and new releases.</p>
</section>

<footer>
  <h2>MKLIONEL MOVIES</h2>
  <p>Entertainment from around the world.</p>
  <p>© 2026 MKLIONEL. All rights reserved.</p>
</footer>

<script>
const API_KEY = "YOUR_TMDB_API_KEY";

const demoMovies = [
  {
    title: "Inception",
    year: "2010",
    poster: "https://image.tmdb.org/t/p/w500/oYuLEt3zVCKq57qu2F8dT7NIa6f.jpg"
  },
  {
    title: "Interstellar",
    year: "2014",
    poster: "https://image.tmdb.org/t/p/w500/gEU2QniE6E77NI6lCU6MxlNBvIx.jpg"
  },
  {
    title: "The Dark Knight",
    year: "2008",
    poster: "https://image.tmdb.org/t/p/w500/qJ2tW6WMUDux911r6m7haRef0WH.jpg"
  },
  {
    title: "Avatar",
    year: "2009",
    poster: "https://image.tmdb.org/t/p/w500/kyeqWdyUXW608qlYkRqosgbbJyK.jpg"
  }
];

const list = document.getElementById("movieList");

function displayMovies(movies) {
  list.innerHTML = "";

  movies.forEach(movie => {
    const card = document.createElement("div");
    card.className = "movie";

    const img = document.createElement("img");
    img.src = movie.poster;
    img.alt = movie.title;

    const title = document.createElement("h3");
    title.textContent = movie.title;

    const year = document.createElement("p");
    year.textContent = movie.year || "";

    card.append(img, title, year);
    list.appendChild(card);
  });
}

displayMovies(demoMovies);

document.getElementById("search").addEventListener("input", e => {
  const query = e.target.value.toLowerCase();

  const filtered = demoMovies.filter(movie =>
    movie.title.toLowerCase().includes(query)
  );

  displayMovies(filtered);
});

// Load worldwide movie information using TMDB
async function loadWorldwideMovies() {
  if (API_KEY === "YOUR_TMDB_API_KEY") return;

  try {
    const response = await fetch(
      "https://api.themoviedb.org/3/discover/movie?include_adult=false&sort_by=popularity.desc",
      {
        headers: {
          Authorization: "Bearer " + API_KEY,
          accept: "application/json"
        }
      }
    );

    if (!response.ok) throw new Error("API error");

    const data = await response.json();

    const movies = data.results.map(movie => ({
      title: movie.title,
      year: movie.release_date?.slice(0, 4),
      poster: movie.poster_path
        ? "https://image.tmdb.org/t/p/w500" + movie.poster_path
        : ""
    })).filter(movie => movie.poster);

    displayMovies(movies);

  } catch (error) {
    console.error("Could not load movies:", error);
  }
}

loadWorldwideMovies();
</script>

</body>
</html>
