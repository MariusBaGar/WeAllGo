"use client";

import { useState, useEffect } from "react";
import { useRouter } from "next/navigation";
import { onAuthStateChanged } from "firebase/auth";
import { auth, db } from "@/lib/firebase";
import { collection, addDoc, Timestamp } from "firebase/firestore";

export default function PublicarViaje() {
  const [user, setUser] = useState<any>(null);
  const [origen, setOrigen] = useState("");
  const [destino, setDestino] = useState("");
  const [fecha, setFecha] = useState("");
  const [plazas, setPlazas] = useState(1);
  const router = useRouter();

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
      if (!currentUser) {
        router.push("/");
      }
    });
    return () => unsubscribe();
  }, [router]);

  const handlePublicar = async () => {
    try {
      await addDoc(collection(db, "viajes"), {
        origen,
        destino,
        fecha: Timestamp.fromDate(new Date(fecha)),
        plazas,
        usuario: user.email,
        creado: Timestamp.now(),
      });
      alert("¡Viaje publicado correctamente!");
      setOrigen("");
      setDestino("");
      setFecha("");
      setPlazas(1);
    } catch (error) {
      alert("Error al publicar el viaje");
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-green-100 to-blue-100 p-4">
      <div className="w-full max-w-md bg-white shadow-xl rounded-2xl p-6 space-y-4">
        <h1 className="text-2xl font-bold text-center text-green-700">Publicar un viaje</h1>

        <input
          type="text"
          placeholder="Origen"
          className="w-full px-4 py-2 border rounded-xl"
          value={origen}
          onChange={(e) => setOrigen(e.target.value)}
        />
        <input
          type="text"
          placeholder="Destino"
          className="w-full px-4 py-2 border rounded-xl"
          value={destino}
          onChange={(e) => setDestino(e.target.value)}
        />
        <input
          type="datetime-local"
          className="w-full px-4 py-2 border rounded-xl"
          value={fecha}
          onChange={(e) => setFecha(e.target.value)}
        />
        <input
          type="number"
          min="1"
          max="6"
          className="w-full px-4 py-2 border rounded-xl"
          value={plazas}
          onChange={(e) => setPlazas(Number(e.target.value))}
        />

        <button
          className="bg-green-600 text-white py-3 rounded-xl w-full hover:bg-green-700 transition"
          onClick={handlePublicar}
        >
          Publicar viaje
        </button>
      </div>
    </div>
  );
}
