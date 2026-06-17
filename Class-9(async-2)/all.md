

### Problem Statement

We want to generate a student's final report card.

To calculate the final grade, we need:

```txt
Attendance Percentage
Assignment Score
Contest Score
```

Each piece of data comes from a different service/database query.

```txt
Attendance Service
Assignment Service
Contest Service
```

All 3 can be fetched simultaneously.

But:

```txt
Can we generate the report if attendance is missing?

NO

Can we generate the report if contest score is missing?

NO
```

The report generation depends on all three results.

---

### Without Promise.all()

```js
async function generateStudentReport(studentId) {

  const attendance =
    await getAttendance(studentId);

  const assignmentScore =
    await getAssignmentScore(studentId);

  const contestScore =
    await getContestScore(studentId);

  const finalScore =
    calculateFinalScore(
      attendance,
      assignmentScore,
      contestScore
    );

  return {
    attendance,
    assignmentScore,
    contestScore,
    finalScore
  };
}
```

Execution:

```txt
Attendance Query
      ↓
Assignment Query
      ↓
Contest Query
      ↓
Generate Report
```

If each query takes 1 second:

```txt
1s + 1s + 1s = 3 seconds
```

---

### With Promise.all()

```js
async function generateStudentReport(studentId) {

  const [
    attendance,
    assignmentScore,
    contestScore
  ] = await Promise.all([
    getAttendance(studentId),
    getAssignmentScore(studentId),
    getContestScore(studentId)
  ]);

  const finalScore =
    calculateFinalScore(
      attendance,
      assignmentScore,
      contestScore
    );

  return {
    attendance,
    assignmentScore,
    contestScore,
    finalScore
  };
}
```

Execution:

```txt
Attendance Query ──────┐
                        │
Assignment Query ──────┼──► Generate Report
                        │
Contest Query ─────────┘
```

All start together.

If each query takes 1 second:

```txt
max(1s, 1s, 1s) = 1 second
```

instead of

```txt
3 seconds
```

---

### The teaching moment

Ask students:

> Can I calculate the final score after only attendance arrives?

```txt
NO
```

> Can I calculate the final score after attendance + assignments arrive?

```txt
NO
```

> When can I calculate it?

```txt
Only after ALL datasets arrive.
```

That's why:

```js
await Promise.all([...])
```


Not because the queries are dependent.

But because the **report generation step depends on all the query results**.

