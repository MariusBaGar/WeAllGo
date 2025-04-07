"use client";

import { useState } from "react";
import { motion } from "framer-motion";
import { createUserWithEmailAndPassword, signInWithEmailAndPassword } from "firebase/auth";
import { auth } from "@/lib/firebase"; // Asegúrate de que esta ruta sea correcta

export default function Home() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSignUp = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
      alert("¡Cuenta creada con éxito! 🎉");
    } catch (error: any) {
      alert("Error al crear cuenta: " + error.message);
    }
  };

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      alert("¡Inicio de sesión exitoso! 🎉");
      // Aquí podrías redirigir a otra página
    } catch (error: any) {
      alert("Error al iniciar sesión: " + error.message);
    }
  };

  return (
    <motion.div
      className="min-h-screen flex items-center justify-center bg-gradient-to-br from-purple-100 to-blue-100 p-4"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
    >
      <div className="w-full max-w-md bg-white shadow-2xl rounded-2xl p-8 space-y-6">
        <h1 className="text-3xl font-bold text-center text-purple-700">
          Bienvenido a WeAllGo
        </h1>
        <p className="text-center text-gray-500">Tu app para compartir trayectos de forma fácil.</p>

        <input
          type="email"
          placeholder="Correo electrónico"
          className="w-full px-4 py-3 border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-purple-300"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <input
          type="password"
          placeholder="Contraseña"
          className="w-full px-4 py-3 border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-purple-300"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <div className="flex flex-col gap-3">
          <button
            className="bg-purple-600 text-white py-3 rounded-xl hover:bg-purple-700 transition"
            onClick={handleLogin}
          >
            Iniciar sesión
          </button>
          <button
            className="bg-white border border-purple-600 text-purple-600 py-3 rounded-xl hover:bg-purple-50 transition"
            onClick={handleSignUp}
          >
            Crear cuenta
          </button>
        </div>
      </div>
    </motion.div>
  );
}
