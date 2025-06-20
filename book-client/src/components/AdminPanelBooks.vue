<script setup>
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const books = ref([]);
const searchText = ref('');
const selectedGenre = ref('Alla');
const genres = ref([]); 
const router = useRouter();

const filteredBooks = computed(() => {
  return books.value.filter(book => {
    const matchesSearch =
      book.title.toLowerCase().includes(searchText.value.toLowerCase()) ||
      book.author.toLowerCase().includes(searchText.value.toLowerCase());

    const matchesGenre =
      selectedGenre.value === 'Alla' || book.genres.includes(selectedGenre.value);

    return matchesSearch && matchesGenre;
  });
});

const goToAddBook = () => {
  router.push('/addbook');
};

const editBook = (id) => {
  router.push(`/editbook/${id}`);
};

const deleteBook = async (id) => {
  if (!confirm('Är du säker?')) return;

  try {
    const response = await fetch(`http://localhost:3000/books/${id}`, {
      method: 'DELETE',
      credentials: 'include'  // här!
    });

    if (response.ok) {
      books.value = books.value.filter(book => book._id !== id);
    } else {
      alert('Misslyckades med att ta bort boken');
    }
  } catch (error) {
    alert('Något gick fel vid borttagningen');
  }
};

onMounted(async () => {
  try {
    const response = await fetch('http://localhost:3000/books');
    const data = await response.json();
    books.value = data;

    // Extrahera un lista med unika genrer
    genres.value = [...new Set(books.value.flatMap(book => book.genres))];
  } catch (error) {
    console.log("Fel vid hämtning av böcker:", error);
  }
});
</script>

<template>
  <main>
    <RouterLink to="/" aria-label="Gå tillbaka till startsidan">
      <span class="material-symbols-outlined" id="arrow_back">arrow_back</span>
    </RouterLink>

    <section class="card" id="allbooks">
      <h2>Hantera böcker</h2>

      <div class="buttons">
        <button @click="goToAddBook" class="buttons">Lägg till en ny bok</button>
      </div>

      <div class="filter-bar">
        <label for="search" class="sr-only">Sök</label>
        <input
          id="search"
          type="text"
          v-model="searchText"
          placeholder="Sök titel eller författare"
          class="filter-input"
        />

        <div class="filter-group">
          <label for="genre-select" class="sr-only">Välj genre</label>
          <select id="genre-select" v-model="selectedGenre" class="filter-select">
            <option>Alla</option>
            <option v-for="genre in genres" :key="genre" :value="genre">{{ genre }}</option>
          </select>
        </div>
      </div>

      

      <div class="card-section">
        <section v-for="book in filteredBooks" :key="book._id" class="card-section div">
          <article>
            <div class="book-info">
              <p class="title">{{ book.title }}</p>
              <p>{{ book.author }}</p>
              <p>{{ book.genres.join(', ') }}</p>
              <p>{{ book.created_at }}</p>

              <div class="edit-delete">
                <span class="material-symbols-outlined action-icon edit-icon" @click="editBook(book._id)" role="button" tabindex="0" aria-label="Ändra bok">edit</span>
                <span class="material-symbols-outlined action-icon delete-icon" @click="deleteBook(book._id)" role="button" tabindex="0" aria-label="Ta bort bok">delete</span>
              </div>
            </div>
          </article>
        </section>
      </div>
    </section>
  </main>
</template>

<style lang="scss" scoped>
main {
  margin-bottom: 20px;
}

#arrow_back {
    color: $green;
    font-weight: 700;
    font-size: 2rem;
    margin-top: 30px;
    margin-left: 20px;
}

.buttons {
  display: flex;
  justify-content: flex-end;
  padding: 0;
  margin: 0;

  button {
    flex: 1;
    max-width: 250px;
    @include primary-button;
  }
}

.card {
  background-color: $green;
  position: relative;
  margin: 10px 20px;
  padding: 10px 20px;
  border-radius: 8px;
}

h2 {
  text-align: center;
  text-transform: uppercase;
  font-family: $H1;
  color: $creamwhite;
  font-weight: 700;
  letter-spacing: $letter_spacing_H2;
  font-size: $mobile_font_size_H2;
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.card-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px; 
}

.card-section div {
  width: 100%; 
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: $creamwhite;
  padding: 15px;
  transition: background-color 0.3s ease;
  border-radius: 8px;
  border: 1px solid $creamwhite;

  @media (max-width: 768px) {
    justify-content: center;
    padding: 10px;
  }
}

article {
  width: 100%; 
  display: flex;
  flex-direction: column;
}

.title {
  font-weight: bold;
  font-size: $mobile_font_size_H3;
}

p {
  width: 100%;
  font-family: $p;
  margin: 0;
  margin-bottom: 10px;
  font-size: $body-font; 
  color: $creamwhite;
  line-height: 1.5;
}

/* Filterbar */
.filter-bar {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 20px;
  align-items: center;

  @media (min-width: 768px) {
    flex-direction: row;
    justify-content: center;
  }
}

.filter-input,
.filter-select {
  padding: 8px 12px;
  border-radius: 6px;
  border: none;
  font-size: 1rem;
  background-color: $creamwhite;
  font-family: $body_font;
}

/* Ändra och ta bort symbolder */
.edit-delete {
  display: flex !important;  
  flex-direction: row !important;
  gap: 10px;
  margin-left: auto;  
  border: none !important; 
  padding: 0 !important;
  background: transparent !important;
}

.action-icon {
  cursor: pointer;
  font-size: 1.5rem;
  color: $creamwhite;
  transition: color 0.3s ease, transform 0.3s ease;
}

.action-icon:hover {
  font-size: 1.7rem;
  color: lighten($creamwhite, 30%);
  transform: scale(1.2);
}
</style>
