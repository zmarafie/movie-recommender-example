
# Movie Recommendation System Example

This project demonstrates a simple movie recommendation system using Python and Google Colab. The system recommends movies based on user preferences and genres.

---

## How to Use This Repository

### Step 1: Download the Notebook
1. Navigate to the file `MovieRecommender.ipynb` in this repository.
   - You can find it listed in the file section of this repository.
2. Click on the file name `MovieRecommender.ipynb`.
3. On the notebook's page, click the **"Download"** button:
   - Look for the **"Download raw"** option, or use the "Download" button in the file viewer.
   - Save the file to your computer.

---

## Running the Notebook in Google Colab

### Step 1: Open Google Colab
1. Go to [Google Colab](https://colab.research.google.com/).
2. Log in with your Google account if not already logged in.

---

### Step 2: Upload the Notebook to Colab
1. In Google Colab, click on **"File"** in the top-left menu.
2. Select **"Upload notebook"**.
3. Click **"Choose file"** and select the `MovieRecommender.ipynb` file you downloaded earlier.
4. Click **"Open"**. The notebook will now load in Google Colab.

---

### Step 3: Run the Notebook
1. To execute a cell, click on it and press the **Play button (▶)** to the left of the cell.
2. If prompted, allow Colab to install any required Python libraries (you might see `!pip install` commands).
3. Follow the instructions in the notebook:
   - Input your favorite movies.
   - View the recommended movies based on the dataset provided.

---

## Example Dataset
The notebook uses a dataset of 100 movies with the following details:
- **Genres**: Action, Drama, Comedy, Sci-Fi, Horror, Romance, Fantasy, Adventure, Animation, Crime, etc.
- **Movies**: Titles include "The Shawshank Redemption," "The Godfather," "The Dark Knight," "Forrest Gump," "The Matrix," etc.
- **Ratings**: Each movie has an average rating between 3.0 and 5.0.

You can customize or expand the dataset directly in the notebook to experiment further.

---

## Example Output

Assuming the user likes these movies:
- **Liked Movies**: `The Shawshank Redemption`, `The Godfather`, `The Dark Knight`

The system outputs the following top 5 recommendations:

| **Title**                           | **Genre**   | **Average Rating** | **Recommendation Score** |
|-------------------------------------|-------------|---------------------|---------------------------|
| Thor: Ragnarok                      | Action      | 4.98               | 5.98                      |
| The Green Mile                      | Drama       | 4.96               | 5.96                      |
| Captain America: The Winter Soldier| Action      | 4.90               | 5.90                      |
| Schindler's List                    | Drama       | 4.84               | 5.84                      |
| Forrest Gump                        | Drama       | 4.84               | 5.84                      |

---

## How to Adjust for New Recommendations

### Step 1: Change the User's Liked Movies
1. In the notebook, locate the following code snippet:
   ```python
   liked_movies = ["The Shawshank Redemption", "The Godfather", "The Dark Knight"]
   ```
2. Replace the movie titles in `liked_movies` with your own preferences. For example:
   ```python
   liked_movies = ["Pulp Fiction", "Schindler's List", "Forrest Gump"]
   ```

### Step 2: Re-run the Notebook
1. After modifying the `liked_movies` list, re-run all the cells in the notebook.
2. The recommendations will update based on the new preferences.

---

## Adjusting the Scoring Formula

The recommendation system uses the following formula to calculate the **Recommendation Score**:

```python
RecommendationScore = GenreScore + AverageRating
```

You can adjust this formula to give more weight to certain factors, such as user preferences (GenreScore) or the movie's general popularity (AverageRating). Below are two examples.

### Example 1: Prioritize User Preferences (GenreScore)
If you want the system to prioritize movies similar to the genres of the user's liked movies, increase the weight of `GenreScore`. For example:

```python
RecommendationScore = (2 * GenreScore) + AverageRating
```

To update the notebook:
1. Locate the section where `RecommendationScore` is calculated:
   ```python
   movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating']
   ```
2. Modify it to:
   ```python
   movies['RecommendationScore'] = (2 * movies['GenreScore']) + movies['Rating']
   ```
3. Re-run all cells in the notebook to apply the new formula.

---

### Example 2: Prioritize Popularity (AverageRating)
If you want to recommend movies that are highly rated across all users, increase the weight of `AverageRating`. For example:

```python
RecommendationScore = GenreScore + (2 * AverageRating)
```

To update the notebook:
1. Locate the section where `RecommendationScore` is calculated:
   ```python
   movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating']
   ```
2. Modify it to:
   ```python
   movies['RecommendationScore'] = movies['GenreScore'] + (2 * movies['Rating'])
   ```
3. Re-run all cells in the notebook to apply the new formula.

---

### Testing Different Formulas
Feel free to experiment with different weights to balance personalization and popularity. For example:

- **Balanced Weighting**:
  ```python
  RecommendationScore = (1.5 * GenreScore) + (1.5 * AverageRating)
  ```
- **Focus on Genre Diversity**:
  ```python
  RecommendationScore = (3 * GenreScore) + AverageRating
  ```

Update the formula in the notebook and re-run all cells to see how the recommendations change!

---
# Advanced Customizations (Optional)
You can modify the scoring formula or introduce additional factors to adjust how recommendations are generated. Below are concepts you can add to the notebook, along with step-by-step instructions.

## Adding Genre Diversity
Encourage recommendations from underrepresented genres by penalizing overrepresented ones.

1. Locate the section where `RecommendationScore` is calculated:
```python
movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating']
```

2. Replace it with:
```python
movies['DiversityPenalty'] = movies['Genre'].map(liked_genres_count) * -0.1
movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating'] + movies['DiversityPenalty']
```

### Explanation:
`DiversityPenalty` adds a small penalty for movies in genres that are already highly represented in the user's liked movies.
This will promote diversity in the recommendations.

---

## Prioritizing Recent Releases
Boost the scores of more recent movies to cater to users who prefer newer content.

1. Add a `ReleaseYear` column to the dataset (if it doesn't exist) with appropriate values.
Locate the `RecommendationScore` calculation and replace it with:

```python
current_year = 2024
movies['RecencyBonus'] = (current_year - movies['ReleaseYear']) * -0.01
movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating'] + movies['RecencyBonus']
```

### Explanation:
Movies released in earlier years receive a small penalty, while recent movies get a bonus.

---

## Adding Popularity Boost
Ensure highly rated movies get an additional boost in recommendations.

Modify the `RecommendationScore` calculation:

```python
movies['PopularityBoost'] = movies['Rating'].apply(lambda r: 0.5 if r > 4.5 else 0)
movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating'] + movies['PopularityBoost']
```

### Explanation:
Movies with an average rating greater than 4.5 receive a boost, prioritizing highly rated content.

---

## Personalizing by Director or Cast
Include user preferences for specific directors or actors.

1. Add a `Director` or `Cast` column to the dataset.
2. Locate the `RecommendationScore` calculation and add:
   
```python
preferred_directors = ["Christopher Nolan", "Steven Spielberg"]
movies['DirectorBonus'] = movies['Director'].apply(lambda d: 0.5 if d in preferred_directors else 0)
movies['RecommendationScore'] = movies['GenreScore'] + movies['Rating'] + movies['DirectorBonus']
```

### Explanation:
Movies directed by the user's favorite directors get a bonus, improving personalization.

---

## Weighted Combination of Factors
Combine multiple factors like genre similarity, recency, and popularity with adjustable weights.

1. Modify the RecommendationScore calculation:
   
```python
movies['RecommendationScore'] = (
    2 * movies['GenreScore'] +
    1.5 * movies['Rating'] +
    1 * movies['RecencyBonus'] +
    0.5 * movies['PopularityBoost']
)
```
2. You may try to play around with the weights and see what differences it makes.
   
### Explanation:
The weights for each factor can be adjusted to balance the importance of each feature.

---

## Troubleshooting

### **Colab Disconnects Frequently**
- Google Colab sessions may timeout after periods of inactivity. Save your progress often by downloading the updated notebook or saving a copy to your Google Drive.

### **Missing Libraries**
- If the notebook shows an error about missing libraries, ensure you run any provided `!pip install` commands.

---

## Saving Your Work
1. **To Save a Copy to Google Drive**:
   - Click **"File" > "Save a copy in Drive"**.
   - This will store the notebook in your Google Drive for future use.

2. **To Download the Updated Notebook**:
   - Click **"File" > "Download .ipynb"** to save it locally.

---

## Feedback
If you encounter any issues or have suggestions for improvement, feel free to contact me or open an issue in this repository.

---

## License
This project is open-source and available under the [MIT License](https://opensource.org/licenses/MIT). Feel free to use and modify it for personal or educational purposes.
