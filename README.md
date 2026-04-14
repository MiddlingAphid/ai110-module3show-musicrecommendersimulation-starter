# 🎵 Music Recommender Simulation

## Project Summary

This project implements **MiddlingMix 1.0**, a content-based music recommender that matches users to songs by scoring genre, mood, and energy closeness. The system loads a catalog of 10 songs from CSV, compares each track against a user's taste profile using weighted scoring rules, and returns a ranked list of recommendations with transparent explanations. It demonstrates how simple mathematical formulas can create the illusion of "smart" recommendations while also revealing where biases and limitations naturally emerge in algorithmic systems.

---

## How The System Works

Each song is represented as an object with ten attributes: id, title, artist, genre, mood, energy (0.0–1.0), tempo_bpm, valence (0.0–1.0), danceability (0.0–1.0), and acousticness (0.0–1.0). A UserProfile stores the user's favorite_genre, favorite_mood, target_energy, and a boolean flag for acoustic preference.

The scoring logic awards **+2.0 points** for a genre match and **+1.0 point** for a mood match. For numerical features like energy, it calculates closeness using `1.0 - abs(song_energy - user_target_energy)` to reward songs that feel energetically similar without penalizing diversity. All scores are summed, then the system sorts songs in descending order and returns the top k results with explanations of why each scored so high.

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Experiments You Tried

- **Adjusting genre weight**: When I dropped the genre weight from 2.0 to 0.5, the rankings completely flipped—suddenly energy became the deciding factor instead of whether a song matched your favorite style. It made me realize how much power one little number has.

- **Adding tempo considerations**: I tried factoring in tempo matching for the lofi-lovers and it actually worked pretty well, cutting out all the fast-paced tracks that technically matched the mood but felt totally wrong.

- **Different user types**: High-energy pop fans got perfect recommendations, but mixing "happy + high energy" with someone wanting sad songs just broke everything—the system couldn't handle contradictory signals and defaulted to mediocre compromises.

---

## Limitations and Risks

- **Tiny catalog problem**: With only ten songs to work from, the system can only shuffle around the same handful of picks. Anyone looking for something outside the stored genres immediately hits a wall.
- **No lyrical understanding**: The algorithm only looks at numbers—BPM, energy levels, metadata tags. It completely misses vibe, storytelling, or why a track matters culturally.
- **Genre gatekeeping**: The +2.0 bonus for genre matches is so strong that it basically locks people into one category, which kills any chance of cross-genre discovery or serendipitous finds.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this

Building this system demonstrated how straightforward mathematical comparisons can effectively translate raw song data into highly personalized user predictions. By assigning specific point values to categorical matches and calculating the distance between numerical features, the model acts as a transparent judge that ranks a catalog based on a predefined "taste profile".

However, the process also revealed that even neutral-looking scoring rules can introduce significant bias if the underlying dataset is unbalanced or if certain weights are tuned too high. For instance, a heavy reliance on genre can create an algorithmic "filter bubble" that ignores relevant music from other categories, potentially leading to an unfair experience for users with diverse or unconventional tastes.

---

## 7. `model_card_template.md`

Combines reflection and model card framing from the Module 3 guidance. :contentReference[oaicite:2]{index=2}  

```markdown
# 🎧 Model Card - Music Recommender Simulation

## 1. Model Name

Give your recommender a name, for example:

> VibeFinder 1.0

MiddlingMix 1.0

---

## 2. Intended Use

- What is this system trying to do
- Who is it for

Example:

> This model suggests 3 to 5 songs from a small catalog based on a user's preferred genre, mood, and energy level. It is for classroom exploration only, not for real users.

This system matches users with a curated selection of songs from a local database by scoring specific stylistic and technical attributes. It functions strictly as a educational tool for students to explore the mechanics of content-based filtering in a controlled environment.

---

## 3. How It Works (Short Explanation)

Describe your scoring logic in plain language.

- What features of each song does it consider
- What information about the user does it use
- How does it turn those into a number

Try to avoid code in this section, treat it like an explanation to a non programmer.

The system acts like a judge that looks at a song’s genre, mood, and intensity to see how well they match your personal taste. It grants bonus points when the music category or feeling is a perfect fit, while also measuring the mathematical "gap" between the track's energy and your preferred vibe. By totaling these points into a single score for every song, it effectively pushes the best matches to the top of your list.

---

## 4. Data

Describe your dataset.

- How many songs are in `data/songs.csv`
- Did you add or remove any songs
- What kinds of genres or moods are represented
- Whose taste does this data mostly reflect

The current library consists of 10 distinct tracks that span a variety of atmospheres, ranging from high-energy pop and intense rock to relaxed jazz and ambient chill. While I kept the original selection intact to maintain a balanced baseline for testing, the diverse mix of lofi and synthwave ensures the system can handle different user vibes. Overall, the dataset leans heavily toward modern digital production styles, reflecting the typical preferences found in study playlists and electronic background music.

---

## 5. Strengths

The recommender excels when a user has a defined taste—searching for "pop + happy + high energy" pulls exactly the tracks you'd expect from that combination. The energy-matching formula adds a clever middle ground between pure categorical matching and full numerical similarity, so related songs can rank well even if they're not in your exact favorite genre. Because every recommendation includes a breakdown of why it scored that way, users can actually understand the logic instead of just trusting a black box.

---

## 6. Limitations and Bias

The system heavily weights genre matches at +2.0 points compared to mood at +1.0, which means it essentially gatekeeps recommendations by category—a user who likes both rock and jazz might miss crossover tracks because they're stuck looking within one silo. The acoustic preference field exists in the user profile but gets completely ignored by the scoring function, so a listener who specifically wants acoustic tracks can't actually express that preference. With only 10 songs in the catalog, any user with even moderately unusual taste ends up seeing the same mediocre picks ranked in slightly different orders.

---

## 7. Evaluation

I tested the system by running it against five different user profiles—a pop enthusiast, a jazz/acoustic lover, someone seeking high-energy dance tracks, and a few hybrid tastes—then manually verified that the top recommendation made intuitive sense for each one. The automated tests in `test_recommender.py` check that songs are returned in descending score order and that recommendations aren't empty, which catches basic breakage but doesn't validate whether the rankings actually feel "right." Because the scoring formula is transparent with explicit point bonuses shown for each match type, I could trace why certain songs ranked higher than others and catch cases where the weighting was off.

---

## 8. Future Work

The biggest win would probably be actually using the `likes_acoustic` field that's sitting unused in the UserProfile—right now it's dead weight. I'd also want to dial down the genre gatekeeping by introducing cross-genre similarity scores or at least letting users express openness to related styles, so someone into pop doesn't instantly reject the one great rock track. Expanding the song catalog would help too, but more importantly, I'd add a diversity filter to the ranking logic so the top 5 picks aren't all variations on the same sound.

---

## 9. Personal Reflection

What surprised me most was how quickly small weighting choices spiral into real effects—twisting just one parameter felt like controlling the entire personality of the recommender. Building this changed my respect for playlist curators, because I now realize that what feels like "random" shuffle on Spotify is probably someone's carefully tuned formula fighting against a thousand edge cases, and I only had to handle a tiny subset. Even though my algorithm can explain every decision it makes, there's a weird emptiness when you realize the system will never understand why someone wants to hear a sad song when they're already sad, or why a "bad" recommendation sometimes leads to discovering your new favorite artist.

