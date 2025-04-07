import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { motion } from "framer-motion";

export default function Home() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSignUp = () => {
    console.log("Crear cuenta con:", email, password);
    // Aquí iría la lógica para crear cuenta (Firebase/Auth0, etc.)
  };

  const handleLogin = () => {
    console.log("Iniciar sesión con:", email, password);
    // Aquí iría la lógica para iniciar sesión
  };

  return (
    <motion.div className="min-h-screen flex items-center justify-center bg-gray-100 p-4">
      <Card className="w-full max-w-md shadow-xl rounded-2xl">
        <CardContent className="p-6 space-y-4">
          <h1 className="text-2xl font-bold text-center">Bienvenido a WeAllGo</h1>
          <Input
            type="email"
            placeholder="Correo electrónico"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
          <Input
            type="password"
            placeholder="Contraseña"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
          <div className="flex flex-col gap-2">
            <Button onClick={handleLogin}>Iniciar sesión</Button>
            <Button variant="outline" onClick={handleSignUp}>
              Crear cuenta
            </Button>
          </div>
        </CardContent>
      </Card>
    </motion.div>
  );
}
