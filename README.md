<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AutoVideo Studio</title>
<style>
body { font-family: Arial; text-align: center; padding: 20px; background: #f7f7f7; }
input { width: 90%; padding: 10px; margin: 10px 0; border-radius: 8px; border: 1px solid #ddd; }
button { padding: 10px 20px; border-radius: 8px; border: none; background: #0066ff; color: white; cursor: pointer; }
#resultat { margin-top: 20px; background: white; padding: 15px; border-radius: 10px; text-align: left; max-width: 600px; margin-left: auto; margin-right: auto; white-space: pre-wrap; }
</style>
</head>
<body>
<h1>Générateur Vidéo YouTube</h1>
<input id="sujet" placeholder="Écris le sujet de ta vidéo">
<br>
<button onclick="generer()">Générer</button>
<div id="resultat"></div>
<script>
async function generer() {
const sujet = document.getElementById("sujet").value;
const zone = document.getElementById("resultat");
zone.textContent = "Patiente… Génération en cours 🚀";
const call = await fetch("/api/generate", {
method: "POST",
headers: {"Content-Type": "application/json"},
body: JSON.stringify({prompt: `Génère une vidéo complète YouTube de 1 minute sur : ${sujet}. Donne : script, voix off, storyboard, prompts IA, description, titres viraux.`})
});
const data = await call.json();
zone.textContent = data.result;
}
</script>
</body>
</html>export default async function handler(req, res) {
const { prompt } = req.body;
try {
const response = await fetch("https://api.openai.com/v1/chat/completions", {
method: "POST",
headers: {
"Content-Type": "application/json",
"Authorization": `Bearer ${process.env.OPENAI_API_KEY}`
},
body: JSON.stringify({
model: "gpt-4o-mini",
messages: [
{ role: "system", content: "Tu es un expert en création de vidéos YouTube automatisées." },
{ role: "user", content: prompt }
]
})
});
const data = await response.json();
return res.status(200).json({ result: data.choices?.[0]?.message?.content || "Erreur de génération." });
} catch (error) {
return res.status(500).json({ error: "Erreur serveur." });
}# Mon-site-vid-o-
Site pour générer des vidéos optinel
