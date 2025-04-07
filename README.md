"use client";

import { useState } from "react";
import { motion } from "framer-motion";

export default function Home() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSignUp = () => {
    console.log("Crear cuenta con:", email, password);
  };

  const handleLogin = () => {
    console.log("Iniciar sesión con:", email, password);
  };

  return (
    <motion.div className="min-h-screen flex items-center justify-center bg-gray-100 p-4">
      <div className="w-full max-w-md bg-white shadow-xl rounded-2xl p-6 space-y-4">
        <h1 className="text-2xl font-bold text-center">Bienvenido a WeAllGo</h1>
        <input
          type="email"
          placeholder="Correo electrónico"
          className="w-full px-4 py-2 border rounded-xl"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <input
          type="password"
          placeholder="Contraseña"
          className="w-full px-4 py-2 border rounded-xl"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <div className="flex flex-col gap-2">
          <button
            className="bg-black text-white py-2 rounded-xl hover:opacity-90"
            onClick={handleLogin}
          >
            Iniciar sesión
          </button>
          <button
            className="border py-2 rounded-xl hover:bg-gray-50"
            onClick={handleSignUp}
          >
            Crear cuenta
          </button>
        </div>
      </div>
    </motion.div>
  );
}
