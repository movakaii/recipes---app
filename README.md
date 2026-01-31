const express = require("express");
const router = express.Router();
const Recipe = require("../models/Recipe"); // Модель рецептов у Алии

// GET /recipes — получить все рецепты
router.get("/", async (req, res) => {
  try {
    const recipes = await Recipe.find();
    res.json(recipes);
  } catch (error) {
    res.status(500).json({ error: "Ошибка при получении рецептов" });
  }
});

// GET /recipes/:id — получить один рецепт
router.get("/:id", async (req, res) => {
  try {
    const recipe = await Recipe.findById(req.params.id);

    if (!recipe) {
      return res.status(404).json({ error: "Рецепт не найден" });
    }

    res.json(recipe);
  } catch (error) {
    res.status(500).json({ error: "Ошибка при получении рецепта" });
  }
});

// POST /recipes — добавить рецепт
router.post("/", async (req, res) => {
  try {
    const newRecipe = new Recipe(req.body);
    const savedRecipe = await newRecipe.save();
    res.status(201).json(savedRecipe);
  } catch (error) {
    res.status(400).json({ error: "Ошибка при создании рецепта" });
  }
});

module.exports = router;
