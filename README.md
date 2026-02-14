import React, { useState, useEffect } from "react"; import { motion } from "framer-motion"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Heart } from "lucide-react";

export default function SanValentinPage() { const [respuesta, setRespuesta] = useState(null); const [noPosition, setNoPosition] = useState({ top: 0, left: 0 }); const [hearts, setHearts] = useState([]);

useEffect(() => { const audio = document.getElementById("bg-music"); if (audio) { audio.volume = 0.4; audio.play().catch(() => {}); }

// corazones flotando
const interval = setInterval(() => {
  setHearts((prev) => [
    ...prev,
    {
      id: Math.random(),
      left: Math.random() * 100,
      size: Math.random() * 20 + 10,
    },
  ]);
}, 800);

return () => clearInterval(interval);

}, []);

const moverBoton = () => { const randomTop = Math.floor(Math.random() * 200) - 100; const randomLeft = Math.floor(Math.random() * 200) - 100; setNoPosition({ top: randomTop, left: randomLeft }); };

return ( <div className="min-h-screen bg-gradient-to-br from-pink-200 via-rose-100 to-red-200 flex items-center justify-center p-6 relative overflow-hidden"> {/* Música */} <audio
id="bg-music"
src="https://www.bensound.com/bensound-music/bensound-romantic.mp3"
loop
/>

{/* Corazones flotando */}
  {hearts.map((heart) => (
    <motion.div
      key={heart.id}
      initial={{ y: "100vh", opacity: 1 }}
      animate={{ y: "-10vh", opacity: 0 }}
      transition={{ duration: 4 }}
      style={{
        position: "absolute",
        left: `${heart.left}%`,
        fontSize: heart.size,
      }}
    >
      💕
    </motion.div>
  ))}

  <motion.div
    initial={{ opacity: 0, scale: 0.9 }}
    animate={{ opacity: 1, scale: 1 }}
    transition={{ duration: 0.6 }}
    className="w-full max-w-md"
  >
    <Card className="rounded-2xl shadow-2xl bg-white">
      <CardContent className="p-6 text-center space-y-4">
        {/* FOTO */}
        <motion.div
          whileHover={{ scale: 1.05 }}
          className="flex justify-center"
        >
          <img
            src="/foto-san-valentin.jpg"
            alt="Mi persona especial"
            className="w-48 h-48 object-cover rounded-2xl shadow-lg border-4 border-rose-300"
          />
        </motion.div>

        <h2 className="text-2xl font-bold text-rose-600">
          Para Ti, Mi Amor 💖
        </h2>

        <h1 className="text-2xl font-bold text-rose-600 flex items-center justify-center gap-2">
          <Heart className="text-rose-500" />
          ¿Quieres ser mi San Valentín?
        </h1>

        {!respuesta && (
          <div className="relative flex justify-center gap-4 pt-6 h-24">
            <Button
              onClick={() => setRespuesta("si")}
              className="bg-rose-500 hover:bg-rose-600 text-white rounded-2xl px-6 text-lg"
            >
              💖 Sí
            </Button>

            <motion.div
              animate={{ top: noPosition.top, left: noPosition.left }}
              transition={{ type: "spring", stiffness: 300 }}
              className="absolute"
            >
              <Button
                onMouseEnter={moverBoton}
                onClick={moverBoton}
                variant="outline"
                className="rounded-2xl px-6 text-lg"
              >
                😢 No
              </Button>
            </motion.div>
          </div>
        )}

        {respuesta === "si" && (
          <motion.div
            initial={{ opacity: 0, scale: 0.5 }}
            animate={{ opacity: 1, scale: 1.2 }}
            transition={{ type: "spring", stiffness: 200 }}
            className="space-y-4 pt-6"
          >
            <p className="text-2xl font-bold text-rose-600">
              ¡Sabía que dirías que sí! 💘✨
            </p>
            <p className="text-lg text-gray-600">
              Prometo hacer este 14 de febrero inolvidable ❤️
            </p>
            <div className="text-4xl animate-bounce">💞💞💞</div>
          </motion.div>
        )}
      </CardContent>
    </Card>
  </motion.div>
</div>

); }
