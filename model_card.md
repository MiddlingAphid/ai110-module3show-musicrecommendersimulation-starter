# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

**MiddlingMix 1.0** — a content-based music recommender using categorical and numerical feature matching.

---

## 2. Intended Use  

This system suggests 3 to 5 songs from a small hand-curated catalog based on a user's preferred genre, mood, and target energy level. It is designed strictly as an educational tool for classroom exploration of how recommendation algorithms work, not for real users or commercial applications.

---

## 3. How the Model Works  

The system acts like a judge that looks at a song's genre, mood, and energy intensity to see how well they match your personal taste. It awards bonus points when the music category or feeling is a perfect fit (+2.0 for genre, +1.0 for mood), while also measuring the mathematical gap between a track's energy and your preferred vibe using the formula `1.0 - abs(song_energy - target_energy)`. By totaling these points for every song, it ranks the catalog and pushes the best matches to the top.

---

## 4. Data  

The current library consists of 10 distinct tracks spanning high-energy pop and rock to relaxed jazz and ambient chill. The dataset includes modern digital production styles heavily represented in study playlists and electronic background music. While the catalog is small and hand-picked, it provides a balanced baseline for testing how the recommender handles different user vibes and genres.

---

## 5. Strengths  

The recommender excels when a user has a defined taste profile—searching for "pop + happy + high energy" pulls exactly the tracks you'd expect from that combination. The energy-matching formula adds a clever middle ground between pure categorical matching and full numerical similarity, so related songs rank well even without matching your favorite genre. Because every recommendation includes a breakdown of why it scored that way, users can understand the logic instead of trusting a black box.

---

## 6. Limitations and Bias 

The system heavily weights genre matches at +2.0 compared to mood at +1.0, which essentially gatekeeps recommendations by category—a user liking both rock and jazz might miss crossover tracks. The acoustic preference field exists in the user profile but gets completely ignored by the scoring function, so listeners who specifically want acoustic music can't express that. With only ten songs in the catalog, any user with unusual taste ends up seeing the same mediocre picks ranked in slightly different orders.

---

## 7. Evaluation  

I tested the system by running it against five different user profiles (pop enthusiast, jazz/acoustic lover, high-energy dance seeker, and hybrid tastes), then manually verified that the top recommendation made intuitive sense for each one. The automated tests in `test_recommender.py` check that songs are returned in descending score order and that recommendations aren't empty, catching basic breakage but not validating whether rankings feel "right." Because the scoring formula is transparent with explicit point bonuses shown for each match type, I could trace why certain songs ranked higher and catch cases where weighting was off.

---

## 8. Future Work  

The biggest win would be actually using the `likes_acoustic` field that's sitting unused in the UserProfile right now. I'd also dial down the genre gatekeeping by introducing cross-genre similarity scores or letting users express openness to related styles, so someone into pop doesn't instantly reject the one great rock track. Adding a diversity filter to the ranking logic would ensure the top 5 picks aren't all variations on the same sound.  

---

## 9. Personal Reflection  

A few sentences about your experience.  

Prompts:  

- What you learned about recommender systems  
- Something unexpected or interesting you discovered  
- How this changed the way you think about music recommendation apps  
