# Student-AI
import express from "express";
import cors from "cors";
import fetch from "node-fetch";

const app = express();
app.use(express.json());
app.use(cors());

const OPENAI_KEY = process.env.OPENAI_API_KEY;

// route
app.post("/student-ai", async (req, res) => {
  const userQuestion = req.body.question;

  try {
    const apiRes = await fetch("https://api.openai.com/v1/chat/completions", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + OPENAI_KEY
      },
      body: JSON.stringify({
        model: "gpt-4o-mini",
        messages: [
          { role: "system", content: "You are a student tutor AI. Explain clearly." },
          { role: "user", content: userQuestion }
        ]
      })
    });

    const data = await apiRes.json();
    res.json({ answer: data.choices[0].message.content });

  } catch (err) {
    res.json({ answer: "Server error: " + err.message });
  }
});

app.listen(3000, () => console.log("AI Server Running"));

