## 07/13/26

Today I learned something new about Postgres.

I'm building FitLog (a workout tracker) as part of a self-directed fullstack bootcamp, and Module 1 (July) was all databases. Two experiments stood out.

**1. RANK() vs. DISTINCT ON for personal records**

My /api/personal-records endpoint used DISTINCT ON + WHERE to grab each user's best lift. It works, but it has a catch: when two rows tie, DISTINCT ON just picks one — whichever your ORDER BY happens to surface first. Fast, but the tie-breaking is silent. No bueno.

I worked with Claude to brainstorm a better approach, and we landed on RANK() OVER (PARTITION BY ...). Instead of collapsing ties, it assigns every row a rank, and tied rows share the same rank. Same goal (top result per group), but now ties are visible instead of hidden.

**2. Full-text search vs. ILIKE**

I ran a comparison on user search across a 250k-row users table, searching the name "Ryan" — once with full-text search, once with ILIKE.

- Full-text search: 200 matches, ~450ms
    
- ILIKE: 1,358 matches, 74ms
    

FTS was slower and matched fewer rows. On paper, the "smarter" tool looked like it lost on every metric. But the match counts told the real story — ILIKE was just doing a substring scan, so it was also catching "Bryan," "Ryannon," anything with those letters in it. FTS was right to exclude those; it just hadn't been given what it needed to do that fast. That's where I learned why indexes matter.

Without an index, FTS recomputes the tsvector for every row, on every search — more expensive than a plain scan, even though it's doing the more correct thing. It has to check every row sequentially, which doesn't hold up at scale. An index is basically a shortcut so FTS doesn't have to look at every row — it can jump straight to what's relevant and skip the rest.

Once I added the index, FTS could actually show what it's capable of: execution time dropped to 0.3ms, roughly 1,500x faster.

Lesson: the "right" tool for the job can still lose badly if you skip the boring infrastructure step. An index isn't an optimization you bolt on later — it's part of whether the tool works as designed at all.

Still early in this bootcamp, but these are the kinds of tradeoffs I never had to reason about as a frontend dev. Enjoying the shift so far.

### 7/14

Today i compared my leadboard SQL vs drizzle ORM. I learned that drizzle will convert my leftJoin to an inner join if i put the WHERE chain after the on chain. In order to keep the LEFTJOIN logic the same i need to use "and()" function and combine the "where()".

```sql
 const result = pool.query(`SELECT users.id, users.name,
	COUNT(workout_sessions.id) AS total_sessions
	 FROM users
	 LEFT JOIN workout_sessions ON users.id = workout_sessions.user_id
	 WHERE workout_sessions.deleted_at IS NULL
	 GROUP BY users.id
	 ORDER BY total_sessions DESC
	 LIMIT 20;`)
```

```js
const result = await db.select({
id: users.id,
user_name: users.name,
total_sessions: count(workout_sessions.id)
}).from(users).leftJoin(workout_sessions, and(eq(users.id, workout_sessions.user_id), isNull(workout_sessions.deleted_at)))
.groupBy(users.id).orderBy(count(workout_sessions.id)).limit(20).toSQL();
```
