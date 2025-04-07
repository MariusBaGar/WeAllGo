"use client";

import { useState, useEffect } from "react";
import { motion } from "framer-motion";
import { createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged } from "firebase/auth";
import { auth } from "@/lib/firebase";
import { useRouter } from "next/navigation";

export default function Home() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [user, setUser] = useState<any>(null);
  const router = useRouter();

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
    });
    return () => unsubscribe();
  }, []);

  const handleSignUp = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
      alert("¡Cuenta creada con éxito!");
    } catch (error: any) {
      alert("Error al crear cuenta: " + error.message);
    }
  };

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      router.push("/publicar");
    } catch (error: any) {
      alert("Error al iniciar sesión: " + error.message);
    }
  };

  const handleLogout = async () => {
    await auth.signOut();
    alert("Sesión cerrada");
  };

  return (
    <motion.div
      className="min-h-screen flex items-center justify-center bg-gradient-to-br from-purple-100 to-blue-100 p-4"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
    >
      <div className="w-full max-w-md bg-white shadow-2xl rounded-2xl p-8 space-y-6">
        {!user ? (
          <>
            <h1 className="text-3xl font-bold text-center text-purple-700">
              Bienvenido a WeAllGo
            </h1>
            <input
              type="email"
              placeholder="Correo electrónico"
              className="w-full px-4 py-3 border border-gray-300 rounded-xl text-black"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
            />
            <input
              type="password"
              placeholder="Contraseña"
              className="w-full px-4 py-3 border border-gray-300 rounded-xl text-black"
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
          </>
        ) : (
          <>
            <h2 className="text-2xl font-semibold text-center text-purple-600">
              ¡Hola, {user.email}!
            </h2>
            <p className="text-center text-gray-500">Estás logueado con éxito.</p>
            <button
              className="bg-red-500 text-white py-3 w-full rounded-xl hover:bg-red-600 transition"
              onClick={handleLogout}
            >
              Cerrar sesión
            </button>
          </>
        )}
      </div>
    </motion.div>
  );
}
