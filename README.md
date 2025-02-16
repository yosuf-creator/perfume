<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Perfume Finder</title>
    <style>
        /* Your previous CSS styles... */
    </style>
</head>
<body>

    <h1>Let’s Find Your Perfect Perfume!</h1>

    <div class="form-container">
        <form id="perfumeForm">
            <label for="gender">What’s your gender?</label>
            <select id="gender">
                <option value="female">Female</option>
                <option value="male">Male</option>
                <option value="unisex">Unisex</option>
            </select>

            <label for="budget">What’s your budget? (in AED)</label>
            <input type="number" id="budget" placeholder="Enter your budget" min="1">

            <label for="favoriteNotes">What are your favorite notes? (Pick as many as you like)</label>
            <div class="checkbox-container" id="favoriteNotes">
                <!-- Checkboxes for favorite notes -->
            </div>
        </form>
    </div>

    <div class="perfume-suggestions">
        <h2>Your Perfume Suggestions</h2>
        <div id="suggestions">
            <p class="loading">We’re finding the best perfumes for you...</p>
        </div>
    </div>

    <div class="footer">
        <p>Need more help? <a href="mailto:support@perfume.com">Contact us</a></p>
    </div>

    <script>
        const form = document.getElementById('perfumeForm');
        const suggestionsContainer = document.getElementById('suggestions');
        const budgetInput = document.getElementById('budget');

        const perfumes = [
            { name: "Chanel No. 5", link: "https://www.google.com/search?q=Chanel+No.+5+perfume", notes: ["rose", "amber"], brand: "Chanel", gender: "female", price: 500 },
            { name: "Dior Sauvage", link: "https://www.google.com/search?q=Dior+Sauvage+perfume", notes: ["pepper", "amber"], brand: "Dior", gender: "male", price: 450 },
            { name: "Tom Ford Black Orchid", link: "https://www.google.com/search?q=Tom+Ford+Black+Orchid+perfume", notes: ["orchid", "patchouli"], brand: "Tom Ford", gender: "unisex", price: 650 },
            { name: "Yves Saint Laurent Libre", link: "https://www.google.com/search?q=Yves+Saint+Laurent+Libre+perfume", notes: ["lavender", "vanilla"], brand: "Yves Saint Laurent", gender: "female", price: 550 },
            // Other perfume entries with price added...
        ];

        form.addEventListener('change', function() {
            updateSuggestions();
        });

        budgetInput.addEventListener('input', function() {
            updateSuggestions();
        });

        budgetInput.addEventListener('keydown', function(event) {
            if (event.key === "Enter") {
                updateSuggestions();
            }
        });

        function updateSuggestions() {
            const gender = document.getElementById('gender').value;
            const favoriteNotes = Array.from(document.querySelectorAll('#favoriteNotes input:checked')).map(checkbox => checkbox.value);
            const budget = parseInt(budgetInput.value) || Infinity;  // If no budget is entered, it allows all perfumes.

            // Show loading message
            suggestionsContainer.innerHTML = '<p class="loading">Hold on, we’re finding your perfect perfumes...</p>';

            // Filter perfumes based on gender, selected notes, and budget
            const filteredPerfumes = perfumes.filter(perfume => {
                const matchesNotes = favoriteNotes.some(note => perfume.notes.includes(note));
                const matchesGender = (perfume.gender === gender || perfume.gender === "unisex");
                const matchesBudget = perfume.price <= budget;
                return matchesNotes && matchesGender && matchesBudget;
            });

            // Display results
            suggestionsContainer.innerHTML = '';
            if (filteredPerfumes.length === 0) {
                suggestionsContainer.innerHTML = '<p class="no-results">Oops, no perfumes match your criteria. Try selecting different notes or increasing your budget!</p>';
            } else {
                filteredPerfumes.forEach(perfume => {
                    const perfumeItem = document.createElement('div');
                    perfumeItem.classList.add('perfume-item');
                    perfumeItem.innerHTML = `<a class="perfume-name" href="${perfume.link}" target="_blank">${perfume.name} - AED ${perfume.price}</a>`;
                    suggestionsContainer.appendChild(perfumeItem);
                });
            }
        }

        // Initial call to display suggestions
        updateSuggestions();
    </script>

</body>
</html>
