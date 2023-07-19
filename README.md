# TEST-NAME-IMDB
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <link rel="stylesheet" href="styles.css">
  <title>Mini IMDB Clone</title>
</head>
<body>
  <div class="container mt-4">
    <div class="form-group">
      <input type="text" id="searchInput" class="form-control" placeholder="Search Movie">
    </div>
    <div id="searchResults" class="row"></div>
  </div>

  <div id="movieDetailsModal" class="modal fade" tabindex="-1" role="dialog">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="movieDetailsTitle"></h5>
          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body" id="movieDetailsBody"></div>
      </div>
    </div>
  </div>

  <script src="scripts.js"></script>
</body>
</html>

CSS Code
/* Add your custom styles here */

/* Style for the movie cards */
.card {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.card-img-top {
  height: 300px;
  object-fit: cover;
}

.card-title {
  font-size: 1.2rem;
  font-weight: bold;
}

/* Style for the movie details modal */
.modal-dialog {
  max-width: 800px;
}

.modal-title {
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
}

.modal-body img {
  width: 100%;
  max-height: 400px;
  object-fit: cover;
  margin-bottom: 10px;
}

.modal-body p {
  margin-bottom: 20px;
}

/* Style for the favorite movies page */
.favorite-movies-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}

.favorite-movie-card {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 10px;
  text-align: center;
}

.favorite-movie-card img {
  height: 200px;
  object-fit: cover;
  margin-bottom: 10px;
}

.favorite-movie-title {
  font-size: 1rem;
  font-weight: bold;
}

.remove-btn {
  color: #fff;
  background-color: #dc3545;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.remove-btn:hover {
  background-color: #c82333;
}

Javascript
document.addEventListener("DOMContentLoaded", function () {
  const apiKey = "YOUR_IMDB_API_KEY"; // Replace this with your IMDB API key
  const searchInput = document.getElementById("searchInput");
  const searchResults = document.getElementById("searchResults");

  // Function to fetch movie data from the API based on the user's input
  async function searchMovies(query) {
    try {
      const response = await fetch(`https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}`);
      const data = await response.json();
      return data.results;
    } catch (error) {
      console.error("Error fetching movie data:", error);
      return [];
    }
  }

  // Function to display search results
  function showSearchResults(movies) {
    searchResults.innerHTML = "";

    movies.forEach(movie => {
      const movieCard = document.createElement("div");
      movieCard.classList.add("col-md-3", "mb-4");
      movieCard.innerHTML = `
        <div class="card">
          <img src="https://image.tmdb.org/t/p/w500/${movie.poster_path}" class="card-img-top" alt="${movie.title}">
          <div class="card-body">
            <h5 class="card-title">${movie.title}</h5>
            <button class="btn btn-primary btn-sm" onclick="viewMovieDetails(${movie.id})">View Details</button>
            <button class="btn btn-secondary btn-sm ml-2" onclick="addToFavorites(${movie.id})">Add to Favorites</button>
          </div>
        </div>
      `;

      searchResults.appendChild(movieCard);
    });
  }

  // Function to handle user input and display search results
  async function handleSearch() {
    const query = searchInput.value.trim();

    if (query.length > 0) {
      const movies = await searchMovies(query);
      showSearchResults(movies);
    } else {
      searchResults.innerHTML = "";
    }
  }

  // Function to view movie details in a modal
  async function viewMovieDetails(movieId) {
    try {
      const response = await fetch(`https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}`);
      const movie = await response.json();
      const modalTitle = document.getElementById("movieDetailsTitle");
      const modalBody = document.getElementById("movieDetailsBody");

      modalTitle.textContent = movie.title;
      modalBody.innerHTML = `
        <img src="https://image.tmdb.org/t/p/w500/${movie.poster_path}" class="img-fluid mb-3" alt="${movie.title}">
        <p>${movie.overview}</p>
        <!-- Add other movie details as needed -->
      `;

      // Show the modal
      $("#movieDetailsModal").modal("show");
    } catch (error) {
      console.error("Error fetching movie details:", error);
    }
  }

  // Function to add a movie to favorites
  function addToFavorites(movieId) {
    // Implement the functionality to add the movie to "My Favourite Movies" list
    // Save the list to Local Storage or Session Storage for persistence
  }

  // Attach event listener for search input
  searchInput.addEventListener("input", handleSearch);
});

