(Files content cropped to 300k characters, download full ingest to see more)
================================================
FILE: README.md
================================================
# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project.



================================================
FILE: eslint.config.js
================================================
import js from '@eslint/js'
import globals from 'globals'
import reactHooks from 'eslint-plugin-react-hooks'
import reactRefresh from 'eslint-plugin-react-refresh'
import { defineConfig, globalIgnores } from 'eslint/config'

export default defineConfig([
  { ignores: ['backend/**'] },
  globalIgnores(['dist']),
  {
    files: ['**/*.{js,jsx}'],
    extends: [
      js.configs.recommended,
      reactHooks.configs.flat.recommended,
      reactRefresh.configs.vite,
    ],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
      parserOptions: {
        ecmaVersion: 'latest',
        ecmaFeatures: { jsx: true },
        sourceType: 'module',
      },
    },
    rules: {
      'no-unused-vars': ['error', { varsIgnorePattern: '^[A-Z_]' }],
    },
  },
])



================================================
FILE: index.html
================================================
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image" href="/src/assets/LOGO.png" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ArteNauta</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>



================================================
FILE: package.json
================================================
{
  "name": "artenauta-frontend",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "preview": "vite preview"
  },
  "dependencies": {
    "@supabase/supabase-js": "^2.106.0",
    "axion": "^0.1.0",
    "axios": "^1.16.1",
    "cors": "^2.8.6",
    "dotenv": "^17.4.2",
    "express": "^5.2.1",
    "firebase": "^12.13.0",
    "react": "^19.2.4",
    "react-dom": "^19.2.4",
    "sweetalert2": "^11.26.24"
  },
  "devDependencies": {
    "@eslint/js": "^9.39.4",
    "@types/react": "^19.2.14",
    "@types/react-dom": "^19.2.3",
    "@vitejs/plugin-react": "^6.0.1",
    "eslint": "^9.39.4",
    "eslint-plugin-react-hooks": "^7.0.1",
    "eslint-plugin-react-refresh": "^0.5.2",
    "globals": "^17.4.0",
    "nodemon": "^3.1.14",
    "vite": "^8.0.1"
  }
}



================================================
FILE: vite.config.js
================================================
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import fs from 'fs';

export default defineConfig(({ command }) => {
  const config = {
    plugins: [react()],
    server: {
      host: 'localhost',
      port: 5173,
    }
  };

  if (
    command === 'serve' && 
    fs.existsSync('./certs/localhost.key') && 
    fs.existsSync('./certs/localhost.crt')
  ) {
    config.server.https = {
      key: fs.readFileSync('./certs/localhost.key'),
      cert: fs.readFileSync('./certs/localhost.crt')
    };
  }

  return config;
});


================================================
FILE: backend/index.js
================================================
import express from "express";
import cors from "cors";

import authRoutes from "./routes/auth.js"
import publicacionesRoutes from "./routes/publicaciones.js";
import usuariosRoutes from "./routes/usuarios.js";
import notificacionesRoutes from "./routes/notificaciones.js";
import comentariosRoutes from "./routes/comentarios.js";
import conversacionesRoutes from "./routes/conversaciones.js";
import estadisticasRoutes from "./routes/estadisticas.js";
import categoriasRoutes from "./routes/categorias.js"
import perfilesRoutes from "./routes/perfiles.js"
import reaccionesRoutes from "./routes/reacciones.js"

const app = express();

app.use(cors());
app.use(express.json());

app.use(notificacionesRoutes);
app.use(authRoutes);
app.use(publicacionesRoutes);
app.use(usuariosRoutes);
app.use(comentariosRoutes);
app.use(conversacionesRoutes);
app.use(estadisticasRoutes);
app.use(categoriasRoutes);
app.use(perfilesRoutes)
app.use(reaccionesRoutes);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Servidor de ArteNauta corriendo en el puerto ${PORT}`);
});


================================================
FILE: backend/package.json
================================================
{
    "name": "backend",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "type": "module",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "nodemon index.js"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {
        "@supabase/supabase-js": "^2.106.1",
        "bcrypt": "^6.0.0",
        "cors": "^2.8.6",
        "dotenv": "^17.4.2",
        "express": "^5.2.1",
        "jsonwebtoken": "^9.0.3",
        "nodemailer": "^9.0.6"
    },
    "devDependencies": {
        "nodemon": "^3.1.14"
    }
}



================================================
FILE: backend/config/supabase.js
================================================
import { createClient } from '@supabase/supabase-js';
import dotenv from 'dotenv';

dotenv.config();

const supabaseUrl = process.env.SUPABASE_URL;
const supabaseKey = process.env.SUPABASE_SERVICE_ROLE_KEY;

export const supabase = createClient(supabaseUrl, supabaseKey);


================================================
FILE: backend/middlewares/verificarToken.js
================================================
import jwt from "jsonwebtoken";

export const verificarToken = (req, res, next) => {
    const authHeader = req.headers["authorization"];
    
    const token = authHeader && authHeader.split(" ")[1];

    if (!token) {
        return res.status(401).json({ mensaje: "Acceso denegado. No hay token." });
    }

    try {
        const secretKey = process.env.JWT_SECRET || "mi_clave_super_secreta_desarrollo";
        const usuarioDecodificado = jwt.verify(token, secretKey);

        req.usuario = usuarioDecodificado;
        
        next();
    } catch (error) {
        return res.status(401).json({ mensaje: "Token inválido o expirado." });
    }
};


================================================
FILE: backend/routes/auth.js
================================================
import express from "express";
import jwt from "jsonwebtoken";
import bcrypt from "bcrypt";
import { supabase } from "../config/supabase.js";
import nodemailer from "nodemailer";

const router = express.Router();

// POST Login de usuarios con JWT
router.post("/auth/login", async (req, res) => {
    const { email, password } = req.body;

    try {
        const { data: usuarioEncontrado, error } = await supabase
            .from("usuarios")
            .select("*")
            .eq("email", email)
            .maybeSingle();

        if (error) throw error;

        if (!usuarioEncontrado) {
            return res.status(401).json({ mensaje: "Correo o contraseña incorrectos" });
        }

        const passwordValida = await bcrypt.compare(password, usuarioEncontrado.clave);

        if (!passwordValida) {
            return res.status(401).json({ mensaje: "Correo o contraseña incorrectos" });
        }

        if (usuarioEncontrado.estado_cuenta === false) {
            return res.status(403).json({
                mensaje: "Tu cuenta ha sido desactivada por un administrador. No puedes iniciar sesión."
            });
        }

        const payload = {
            id_usuario: usuarioEncontrado.id_usuario,
            id_rol: usuarioEncontrado.id_rol
        };

        const secretKey = process.env.JWT_SECRET || "mi_clave_super_secreta_desarrollo";
        const token = jwt.sign(payload, secretKey, { expiresIn: "5h" });

        res.status(200).json({
            mensaje: "Inicio de sesión exitoso",
            token: token,
            usuario: {
                id_usuario: usuarioEncontrado.id_usuario,
                nombre: usuarioEncontrado.nombre,
                email: usuarioEncontrado.email,
                id_rol: usuarioEncontrado.id_rol
            }
        });

    } catch (error) {
        console.error("Error en el login:", error);
        res.status(500).json({ mensaje: "Error interno del servidor" });
    }
});

// POST Solicitar recuperación de contraseña
router.post("/auth/forgot-password", async (req, res) => {
    const { email } = req.body;

    try {
        const { data: usuarioEncontrado, error } = await supabase
            .from("usuarios")
            .select("*")
            .eq("email", email)
            .maybeSingle();

        if (error) throw error;

        // Siempre respondemos éxito por seguridad, para no revelar qué correos están registrados
        if (!usuarioEncontrado) {
            return res.status(200).json({ mensaje: "Si el correo está registrado, recibirás un enlace de recuperación." });
        }

        const secretKey = process.env.JWT_SECRET || "mi_clave_super_secreta_desarrollo";
        const token = jwt.sign({ email }, secretKey, { expiresIn: '15m' });

        // IMPORTANTE: Tu frontend está corriendo con HTTPS según tu vite.config.js
        const frontUrl = process.env.FRONTEND_URL || "https://localhost:5173";
        const resetLink = `${frontUrl}/restablecer-clave?token=${token}`;

        const transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: process.env.SMTP_USER,
                pass: process.env.SMTP_PASS
            }
        });

        await transporter.sendMail({
            from: `"ArteNauta Soporte" <${process.env.SMTP_USER}>`,
            to: email,
            subject: 'Restablecer contraseña - ArteNauta',
            html: `
                <div style="font-family: Arial, sans-serif; color: #333; max-width: 600px; margin: 0 auto; border: 1px solid #e5e7eb; border-radius: 8px; padding: 20px;">
                    <h2 style="color: #0891b2; text-align: center;">Recuperación de Contraseña</h2>
                    <p>Hola <strong>${usuarioEncontrado.nombre}</strong>,</p>
                    <p>Has solicitado restablecer tu contraseña en ArteNauta.</p>
                    <p>Haz clic en el siguiente botón para crear una nueva contraseña. Este enlace es válido por 15 minutos:</p>
                    <div style="text-align: center; margin: 30px 0;">
                        <a href="${resetLink}" style="background-color: #0891b2; color: #ffffff; padding: 12px 24px; text-decoration: none; border-radius: 6px; font-weight: bold; display: inline-block;">Restablecer Contraseña</a>
                    </div>
                    <p style="font-size: 14px; color: #6b7280;">Si no solicitaste este cambio, puedes ignorar este correo sin problema.</p>
                    <hr style="border: none; border-top: 1px solid #e5e7eb; margin: 20px 0;" />
                    <p style="font-size: 12px; color: #9ca3af; text-align: center;">El equipo de ArteNauta</p>
                </div>
            `
        });

        res.status(200).json({ mensaje: "Si el correo está registrado, recibirás un enlace de recuperación." });
    } catch (error) {
        console.error("Error en forgot-password:", error);
        res.status(500).json({ mensaje: "Error al procesar la solicitud de recuperación." });
    }
});

// POST Restablecer contraseña con el token
router.post("/auth/reset-password", async (req, res) => {
    const { token, newPassword } = req.body;

    try {
        const secretKey = process.env.JWT_SECRET || "mi_clave_super_secreta_desarrollo";
        
        let decodificado;
        try {
            decodificado = jwt.verify(token, secretKey);
        } catch (error) {
            return res.status(400).json({ mensaje: "El enlace es inválido o ha expirado. Por favor, solicita uno nuevo." });
        }

        const email = decodificado.email;

        // Encriptar la nueva clave
        const saltRounds = 10;
        const hashedPassword = await bcrypt.hash(newPassword, saltRounds);

        // Actualizar el usuario en Supabase
        const { error } = await supabase
            .from("usuarios")
            .update({ clave: hashedPassword })
            .eq("email", email);

        if (error) throw error;

        res.status(200).json({ mensaje: "Tu contraseña ha sido actualizada correctamente." });
    } catch (error) {
        console.error("Error en reset-password:", error);
        res.status(500).json({ mensaje: "Error interno al actualizar la contraseña." });
    }
});

export default router;


================================================
FILE: backend/routes/categorias.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();

// GET: Obtener todas las categorías fijas ordenadas alfabéticamente
router.get("/categorias", verificarToken, async (req, res) => {
    try {
        const { buscar } = req.query;

        let query = supabase
            .from("categorias")
            .select("*")
            .order("nombre_categoria", { ascending: true });

        if (buscar) {
            query = query.ilike("nombre_categoria", `%${buscar}%`);
        }
        const { data, error } = await query;

        if (error) throw error;
        res.status(200).json(data || []);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// POST: Crear categorias
router.post("/categorias", verificarToken, async (req, res) => {
    try {
        const { nombre_categoria, descripcion } = req.body;

        if (!nombre_categoria || !descripcion) {
            return res
                .status(400)
                .json({
                    error: "Faltan campos obligatorios (Nombre Categoria, Descripcion)",
                });
        }

        const { data, error } = await supabase
            .from("categorias")
            .insert([
                {
                    nombre_categoria,
                    descripcion,
                },
            ])
            .select(`*`);

        if (error) throw error;

        if (!data || data.length === 0) {
            throw new Error("No se pudo obtener la categoria creada.");
        }

        res.status(201).json(data[0]);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// PATCH: Editar categorias por su ID
router.patch("/categorias/:id", verificarToken, async (req, res) => {
    try {
        const { id } = req.params;
        const { nombre_categoria, descripcion } = req.body;

        const { data, error } = await supabase
            .from("categorias")
            .update({
                nombre_categoria: nombre_categoria,
                descripcion: descripcion,
            })
            .eq("id_categoria", id)
            .select();
        if (error) throw error;
        if (data.length === 0) {
            return res.status(404).json({ error: "Categoría no encontrada" });
        }

        res.status(200).json({
            mensaje: "Categoría actualizada con éxito",
            categoria: data[0],
        });
    } catch (error) {
        console.error(error);
        res.status(500).json({ error: error.message });
    }
});

export default router;



================================================
FILE: backend/routes/comentarios.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();

// GET: Obtener comentarios de una publicación específica
router.get("/comentarios/:id_publicacion", verificarToken, async (req, res) => {
    try {
        const { id_publicacion } = req.params;

        const { data, error } = await supabase
            .from("comentarios")
            .select(`
                *,
                usuarios (
                    id_usuario,
                    nombre
                )
            `)
            .eq("id_publicacion", id_publicacion)
            .order("fecha_comentario", { ascending: true });

        if (error) throw error;

        res.status(200).json(data || []);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// POST: Guardar un nuevo comentario
router.post("/comentarios", verificarToken, async (req, res) => {
    try {
        const { id_publicacion, id_usuario_final, contenido } = req.body;

        if (!id_publicacion || !id_usuario_final || !contenido) {
            return res.status(400).json({ error: "Faltan campos requeridos (id_publicacion, id_usuario_final, contenido)" });
        }

        const { data, error } = await supabase
            .from("comentarios")
            .insert([
                {
                    id_publicacion,
                    id_usuario_final,
                    contenido,
                    fecha_comentario: new Date().toISOString(),
                },
            ])
            .select(`
                *,
                usuarios (
                    id_usuario,
                    nombre
                )
            `);

        if (error) throw error;

        if (!data || data.length === 0) {
            throw new Error("No se pudo obtener el comentario insertado.");
        }

        res.status(201).json(data[0]);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// GET: Obtener TODOS los comentarios de la plataforma (Para la tabla de Admin)
router.get("/admin/comentarios", verificarToken, async (req, res) => {
    try {
        const { data, error } = await supabase
            .from("comentarios")
            .select(`
                *,
                usuarios (
                    id_usuario,
                    nombre,
                    email
                ),
                publicaciones (
                    id_publicacion,
                    titulo
                )
            `)
            .order("fecha_comentario", { ascending: false }); // Los más recientes primero

        if (error) throw error;

        res.status(200).json(data || []);
    } catch (error) {
        console.error("Error en GET /admin/comentarios:", error);
        res.status(500).json({ error: error.message });
    }
});

// DELETE: Eliminar un comentario específico por su ID
router.delete("/comentarios/:id_comentario", verificarToken, async (req, res) => {
    try {
        const { id_comentario } = req.params;
        const { error } = await supabase
            .from("comentarios")
            .delete()
            .eq("id_comentario", id_comentario);

        if (error) throw error;

        res.status(200).json({ mensaje: "Comentario eliminado con éxito" });
    } catch (error) {
        console.error("Error en DELETE /comentarios/:id_comentario:", error);
        res.status(500).json({ error: error.message });
    }
});

export default router;



================================================
FILE: backend/routes/conversaciones.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();

// GET Buscar usuarios para iniciar conversacion
router.get("/usuarios/buscar", verificarToken, async (req, res) => {
    try {
        const { email, correo } = req.query;
        const targetEmail = email || correo;
        if (!targetEmail) return res.status(400).json({ error: "Se requiere un correo" });

        const { data, error } = await supabase
            .from("usuarios")
            .select("id_usuario, nombre, apellido, email")
            .eq("email", targetEmail)
            .maybeSingle();

        if (error) throw error;
        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// GET Buscar conversacion de un ID
router.get("/conversaciones/usuario/:id_usuario", verificarToken, async (req, res) => {
    try {
        const id_usuario = parseInt(req.params.id_usuario);
        if (!id_usuario) return res.status(400).json({ error: "id_usuario inválido" });

        const { data: misParticipaciones, error: err1 } = await supabase
            .from("participantes")
            .select("id_conversacion")
            .eq("id_usuario", id_usuario);

        if (err1) throw err1;
        if (!misParticipaciones || misParticipaciones.length === 0) return res.status(200).json([]);

        const idsConversaciones = misParticipaciones.map(p => p.id_conversacion);

        const { data: otrosParticipantes, error: err2 } = await supabase
            .from("participantes")
            .select(`
                id_conversacion,
                id_usuario,
                usuarios (id_usuario, nombre, apellido)
            `)
            .in("id_conversacion", idsConversaciones)
            .neq("id_usuario", id_usuario);

        if (err2) throw err2;

        const resultado = otrosParticipantes.map(p => ({
            id_conversacion: p.id_conversacion,
            id_usuario_otro: p.id_usuario,
            nombre_otro: p.usuarios?.nombre
                ? `${p.usuarios.nombre} ${p.usuarios.apellido || ""}`.trim()
                : "Usuario"
        }));

        res.status(200).json(resultado);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Iniciar la conversacion con Validacion de existencia
router.post("/conversaciones", verificarToken, async (req, res) => {
    try {
        const { id_usuario_1, id_usuario_2 } = req.body;
        if (!id_usuario_1 || !id_usuario_2) return res.status(400).json({ error: "Faltan campos" });

        const { data: part1, error: err1 } = await supabase
            .from("participantes").select("id_conversacion").eq("id_usuario", id_usuario_1);
        if (err1) throw err1;

        if (part1 && part1.length > 0) {
            const ids = part1.map(p => p.id_conversacion);
            const { data: part2, error: err2 } = await supabase
                .from("participantes").select("id_conversacion").in("id_conversacion", ids).eq("id_usuario", id_usuario_2);
            if (err2) throw err2;
            if (part2 && part2.length > 0) return res.status(200).json({ id_conversacion: part2[0].id_conversacion, existe: true });
        }

        const { data: nuevaConv, error: err3 } = await supabase
            .from("conversaciones").insert({ fecha_creacion: new Date().toISOString() }).select("id_conversacion").single();
        if (err3) throw err3;

        const { error: err4 } = await supabase.from("participantes").insert([
            { id_conversacion: nuevaConv.id_conversacion, id_usuario: id_usuario_1 },
            { id_conversacion: nuevaConv.id_conversacion, id_usuario: id_usuario_2 }
        ]);
        if (err4) throw err4;

        res.status(201).json({ id_conversacion: nuevaConv.id_conversacion, existe: false });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// GET Muestra mensajes de una conversacion
router.get("/mensajes/:id_conversacion", verificarToken, async (req, res) => {
    try {
        const { id_conversacion } = req.params;
        const { data, error } = await supabase
            .from("mensajes").select("*").eq("id_conversacion", id_conversacion).order("fecha_envio", { ascending: true });
        if (error) throw error;
        res.status(200).json(data || []);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// POST Crea notificaciones de los mensajes
router.post("/mensajes", verificarToken, async (req, res) => {
    try {
        const { id_conversacion, id_usuario, contenido } = req.body;

        if (!id_conversacion || !id_usuario || !contenido) {
            return res.status(400).json({ error: "Datos incompletos" });
        }

        const { data: nuevoMensaje, error: errorMensaje } = await supabase
            .from("mensajes")
            .insert([{ 
                id_conversacion, 
                id_usuario, 
                contenido, 
                fecha_envio: new Date().toISOString() 
            }])
            .select()
            .single();

        if (errorMensaje) throw errorMensaje;

        const { data: otrosParticipantes, error: errorParticipantes } = await supabase
            .from("participantes")
            .select("id_usuario")
            .eq("id_conversacion", id_conversacion)
            .neq("id_usuario", id_usuario); 

        if (errorParticipantes) throw errorParticipantes;

        if (otrosParticipantes && otrosParticipantes.length > 0) {
            const idReceptor = otrosParticipantes[0].id_usuario;

            const { error: errorNotif } = await supabase
                .from("notificaciones")
                .insert({
                    id_usuario: idReceptor, 
                    asunto: "Tienes un nuevo mensaje en el chat",
                    tipo_notificacion: "Mensaje", 
                    fecha_notificacion: new Date().toISOString()
                });

            if (errorNotif) {
                console.error("Aviso: El mensaje se envió pero la notificación falló:", errorNotif.message);
            }
        }

        res.status(201).json(nuevoMensaje);

    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

export default router;


================================================
FILE: backend/routes/estadisticas.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js"; 

const router = express.Router();

// GET: Obtener métricas consolidadas para el Dashboard de Admin
router.get("/dashboard/estadisticas", verificarToken, async (req, res) => {
    try {
        const [
            usersCount, 
            artistsCount, 
            postsCount, 
            commentsCount
        ] = await Promise.all([
            supabase.from("usuarios").select("*", { count: "exact", head: true }),

            supabase.from("usuarios").select("*", { count: "exact", head: true }).eq("id_rol", 2),

            supabase.from("publicaciones").select("*", { count: "exact", head: true }),

            supabase.from("comentarios").select("*", { count: "exact", head: true })
        ]);

        if (usersCount.error) throw usersCount.error;
        if (artistsCount.error) throw artistsCount.error;
        if (postsCount.error) throw postsCount.error;
        if (commentsCount.error) throw commentsCount.error;

        res.status(200).json({
            totalUsuarios: usersCount.count || 0,
            totalArtistas: artistsCount.count || 0,
            totalPublicaciones: postsCount.count || 0,
            totalComentarios: commentsCount.count || 0
        });

    } catch (error) {
        console.error("Error en endpoint estadisticas:", error);
        res.status(500).json({ error: error.message });
    }
});

export default router;


================================================
FILE: backend/routes/notificaciones.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();


//Metodos GET

// GET — obtener notificaciones de un usuario
router.get("/notificaciones", verificarToken, async (req, res) => {
    try {
        const { id_usuario } = req.query;

        if (!id_usuario) {
            return res.status(400).json({ error: "id_usuario es requerido" });
        }

        const { data, error } = await supabase
            .from("notificaciones")
            .select("*")
            .eq("id_usuario", id_usuario)
            .order("fecha_notificacion", { ascending: false })
            .limit(20);

        if (error) throw error;

        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// GET — obtener solicitudes pendientes (solo admin) (funciona)

router.get("/notificaciones/solicitudes", verificarToken, async (req, res) => {
    try {
        const { data, error } = await supabase
            .from("solicitudes")
            .select("*, usuarios(nombre, apellido)")
            .eq("tipo_solicitud", "artista")
            .eq("estado_solicitud", "Pendiente")
            .order("fecha_solicitud", { ascending: false });

        if (error) throw error;

        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});


//Metodos PATCH

// PATCH — aprobar solicitud
router.patch("/notificaciones/solicitudes/:id/aprobar", verificarToken, async (req, res) => {
    try {
        const { id } = req.params;

        console.log("Intentando aprobar la solicitud con ID:", id);

        // 1. Buscar la solicitud para obtener el id_usuario automáticamente
        const { data: solicitud, error: errorBusqueda } = await supabase
            .from("solicitudes")
            .select("id_usuario")
            .eq("id_solicitud", id)
            .single();

        if (errorBusqueda || !solicitud) {
            return res.status(404).json({ error: "La solicitud no existe o ya fue procesada." });
        }

        const id_usuario = solicitud.id_usuario;

        // 2. Actualizar estado de la solicitud a Aceptada
        const { data, error } = await supabase
            .from("solicitudes")
            .update({ estado_solicitud: "Aceptada" })
            .eq("id_solicitud", id)
            .select();

        if (error) throw error;

        if (!data || data.length === 0) {
            return res.status(404).json({
                error: `No se encontró ninguna solicitud donde id_solicitud sea igual a ${id}`
            });
        }

        // 3. Cambiar rol del usuario a Artista (2)
        const { error: errorRol } = await supabase
            .from("usuarios")
            .update({ id_rol: 2 })
            .eq("id_usuario", id_usuario);
        if (errorRol) throw errorRol;

        // 4. Crear notificación para el usuario
        const { error: errorNotif } = await supabase
            .from("notificaciones")
            .insert({
                id_usuario,
                asunto: "¡Tu solicitud para ser artista fue aprobada!",
                tipo_notificacion: "solicitud_aprobada",
                fecha_notificacion: new Date().toISOString(),
            });
        if (errorNotif) throw errorNotif;

        res.status(200).json({
            mensaje: "¡Solicitud aprobada, rol actualizado y notificación enviada!",
            solicitud: data[0]
        });

    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});


// PATCH — aprobar solicitud (Adaptado)
router.patch("/notificaciones/solicitudes/:id/aprobar", verificarToken, async (req, res) => {
    try {
        const { id } = req.params;

        // 1. Buscar la solicitud para obtener el id_usuario automáticamente de la DB
        const { data: solicitud, error: errorBusqueda } = await supabase
            .from("solicitudes")
            .select("id_usuario")
            .eq("id_solicitud", id)
            .single();

        if (errorBusqueda || !solicitud) {
            return res.status(404).json({ error: "La solicitud no existe o ya fue procesada." });
        }

        const id_usuario = solicitud.id_usuario;

        // 2. Actualizar estado de la solicitud a Aceptada
        const { data, error } = await supabase
            .from("solicitudes")
            .update({ estado_solicitud: "Aceptada" })
            .eq("id_solicitud", id)
            .select();

        if (error) throw error;

        // 3. Cambiar rol del usuario a Artista (2)
        const { error: errorRol } = await supabase
            .from("usuarios")
            .update({ id_rol: 2 })
            .eq("id_usuario", id_usuario);
        if (errorRol) throw errorRol;

        // 4. Crear notificación (Protegido por si el ENUM falla)
        let notificacionEstado = "Creada correctamente";
        try {
            const { error: errorNotif } = await supabase
                .from("notificaciones")
                .insert({
                    id_usuario,
                    asunto: "¡Tu solicitud para ser artista fue aprobada!",
                    tipo_notificacion: "solicitud_aprobada", // Si este falla, el try/catch lo atrapa sin romper la ruta
                    fecha_notificacion: new Date().toISOString(),
                });
            if (errorNotif) throw errorNotif;
        } catch (errNotif) {
            console.log("Aviso: No se creó la fila de notificación por conflicto de ENUM, pero el rol y la solicitud sí se guardaron.");
            notificacionEstado = "No creada (Revisar ENUM en Supabase)";
        }

        res.status(200).json({
            mensaje: "¡Solicitud aprobada y rol actualizado con éxito!",
            notificacion: notificacionEstado,
            solicitud: data[0]
        });

    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});


// PATCH — rechazar solicitud (Adaptado)
router.patch("/notificaciones/solicitudes/:id/rechazar", verificarToken, async (req, res) => {
    try {
        const { id } = req.params;

        // 1. Buscar la solicitud para obtener el id_usuario automáticamente de la DB
        const { data: solicitud, error: errorBusqueda } = await supabase
            .from("solicitudes")
            .select("id_usuario")
            .eq("id_solicitud", id)
            .single();

        if (errorBusqueda || !solicitud) {
            return res.status(404).json({ error: "La solicitud no existe o ya fue procesada." });
        }

        const id_usuario = solicitud.id_usuario;

        // 2. Actualizar estado de la solicitud a Rechazada
        const { error: e1 } = await supabase
            .from("solicitudes")
            .update({ estado_solicitud: "Rechazada" })
            .eq("id_solicitud", id);
        if (e1) throw e1;

        // 3. Crear notificación (Protegido por si el ENUM falla)
        let notificacionEstado = "Creada correctamente";
        try {
            const { error: e2 } = await supabase
                .from("notificaciones")
                .insert({
                    id_usuario,
                    asunto: "Tu solicitud para ser artista fue rechazada.",
                    tipo_notificacion: "solicitud_rechazada", // Si saca error de ENUM, se salta al catch sin tumbar la petición
                    fecha_notificacion: new Date().toISOString(),
                });
            if (e2) throw e2;
        } catch (errNotif) {
            console.log("Aviso: No se creó la notificación por conflicto de ENUM, pero la solicitud se rechazó con éxito.");
            notificacionEstado = "No creada (Revisar ENUM en Supabase)";
        }

        res.status(200).json({ 
            mensaje: "Solicitud rechazada correctamente en Supabase.",
            notificacion: notificacionEstado
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

//Metodos POST
// Crear una nueva solicitud (POST)
router.post('/notificaciones/solicitudes', verificarToken, async (req, res) => {
    const { tipo_solicitud, id_usuario } = req.body;

    try {
        const { data, error } = await supabase
            .from('solicitudes')
            .insert([
                { 
                    tipo_solicitud, 
                    id_usuario, 
                    estado_solicitud: 'Pendiente' // Se crea automáticamente en "Pendiente"
                }
            ])
            .select();

        if (error) {
            return res.status(400).json({ error: error.message });
        }

        res.status(201).json({
            mensaje: "¡Solicitud creada con éxito!",
            solicitud: data[0]
        });

    } catch (error) {
        res.status(500).json({ error: "Error interno del servidor" });
    }
});

//Metodos DELETE

// DELETE — eliminar una notificación específica
router.delete("/notificaciones/:id", verificarToken, async (req, res) => {
    try {
        const { id } = req.params; // ID de la notificación desde la URL

        const { error } = await supabase
            .from("notificaciones")
            .delete()
            .eq("id_notificacion", id); // Cambia "id_notificacion" por el nombre exacto de tu llave primaria

        if (error) throw error;

        res.status(200).json({ mensaje: "Notificación eliminada correctamente en Supabase." });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});


// DELETE — eliminar una solicitud específica 
router.delete("/solicitudes/:id", verificarToken, async (req, res) => {
    try {
        const { id } = req.params; // ID de la solicitud desde la URL

        const { error } = await supabase
            .from("solicitudes")
            .delete()
            .eq("id_solicitud", id); // Elimina la fila que coincida con el ID

        if (error) throw error;

        res.status(200).json({ mensaje: "Solicitud eliminada correctamente de Supabase." });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});


export default router;  


================================================
FILE: backend/routes/perfiles.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();

// GET: obtener el perfil completo del usuario autenticado (usuario + perfil, si existe)
router.get("/perfil", verificarToken, async (req, res) => {
    try {
        const id_usuario = req.usuario.id_usuario;

        const { data, error } = await supabase
            .from("usuarios")
            .select(`
                id_usuario,
                nombre,
                apellido,
                email,
                telefono,
                perfiles (
                    id_perfil,
                    foto_perfil,
                    descripcion,
                    ocupacion
                )
            `)
            .eq("id_usuario", id_usuario)
            .single();

        if (error) throw error;
        const perfil = data.perfiles || null;

        res.status(200).json({
            id_usuario: data.id_usuario,
            nombre: data.nombre,
            apellido: data.apellido,
            email: data.email,
            telefono: data.telefono,
            foto_perfil: perfil?.foto_perfil ?? null,
            descripcion: perfil?.descripcion ?? "",
            ocupacion: perfil?.ocupacion ?? "",
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// PATCH: crear o actualizar el perfil extendido del usuario
router.patch("/perfil", verificarToken, async (req, res) => {
    try {
        const id_usuario = req.usuario.id_usuario;
        const { descripcion, ocupacion } = req.body;

        const { data, error } = await supabase
            .from("perfiles")
            .upsert(
                { id_usuario, descripcion, ocupacion },
                { onConflict: "id_usuario" }
            )
            .select()
            .single();

        if (error) throw error;

        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// PATCH: actualizar nombre, apellido y teléfono del usuario autenticado
router.patch("/perfil/usuario", verificarToken, async (req, res) => {
    try {
        const id_usuario = req.usuario.id_usuario;
        const { nombre, apellido, telefono } = req.body;

        const { data, error } = await supabase
            .from("usuarios")
            .update({ nombre, apellido, telefono })
            .eq("id_usuario", id_usuario)
            .select(`id_usuario, nombre, apellido, email, telefono, id_rol`)
            .single();

        if (error) throw error;

        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

export default router;


================================================
FILE: backend/routes/publicaciones.js
================================================
import express from "express";
import axios from "axios";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();

router.get("/Muro-Publicaciones", verificarToken, async (req, res) => {
    try {
        const { buscar, ordenFecha, ordenLikes } = req.query;

        let consulta = supabase.from("publicaciones").select(`
                *,
                usuarios (
                    id_usuario,
                    nombre,
                    email
                ),
                reacciones (
                    id_reaccion
                )
            `);

        if (buscar && buscar.trim() !== "") {
            consulta = consulta.or(
                `titulo.ilike.%${buscar}%,descripcion.ilike.%${buscar}%`,
            );
        }

        if (!ordenLikes) {
            if (ordenFecha) {
                consulta = consulta.order("fecha_publicacion", {
                    ascending: ordenFecha === "asc",
                });
            } else {
                consulta = consulta.order("fecha_publicacion", {
                    ascending: false,
                });
            }
        }

        const { data, error } = await consulta;

        if (error) throw error;

        const publicacionesConLikes = (data || []).map((pub) => ({
            ...pub,
            totalLikes: pub.reacciones?.length || 0,
        }));

        // ordenar por likes
        if (ordenLikes) {
            publicacionesConLikes.sort((a, b) =>
                ordenLikes === "asc"
                    ? a.totalLikes - b.totalLikes
                    : b.totalLikes - a.totalLikes,
            );
        }

        res.status(200).json(publicacionesConLikes);
    } catch (error) {
        res.status(500).json({
            error: error.message,
        });
    }
});

// GET: Obtener publicaciones de un artista específico
router.get(
    "/publicaciones/artista/:id_artista",
    verificarToken,
    async (req, res) => {
        try {
            const { id_artista } = req.params;
            const { data, error } = await supabase
                .from("publicaciones")
                .select("*")
                .eq("id_usuario_artista", id_artista)
                .order("fecha_publicacion", { ascending: false });

            if (error) throw error;

            res.status(200).json(data || []);
        } catch (error) {
            res.status(500).json({ error: error.message });
        }
    },
);

// POST: Crear una nueva publicación + api sightengine
router.post("/publicaciones", verificarToken, async (req, res) => {
    try {
        const {
            titulo,
            contenido,
            descripcion,
            id_usuario_artista,
            id_categoria,
        } = req.body;
        if (!titulo || !id_usuario_artista) {
            return res
                .status(400)
                .json({
                    error: "Faltan campos obligatorios (titulo, id_usuario_artista)",
                });
        }

        // validacion con Sightengine
        if (contenido && contenido.trim() !== "") {
            try {
                const MODELS =
                    "nudity-2.1,weapon,alcohol,recreational_drug,gore-2.0,violence,self-harm";
                const sightengineRes = await axios.get(
                    "https://api.sightengine.com/1.0/check.json",
                    {
                        params: {
                            url: contenido,
                            models: MODELS,
                            api_user: process.env.SIGHTENGINE_USER,
                            api_secret: process.env.SIGHTENGINE_SECRET,
                        },
                    },
                );

                const dataApi = sightengineRes.data;

                if (dataApi.status === "failure") {
                    return res.status(522).json({
                        error: `Error de Sightengine: ${dataApi.error.message}`,
                    });
                }

                const nudity = dataApi.nudity?.raw ?? 0;
                const violence = dataApi.violence?.prob ?? 0;
                const gore = dataApi.gore?.prob ?? 0;
                const drugs = dataApi.recreational_drug?.prob ?? 0;
                if (
                    nudity > 0.5 ||
                    violence > 0.5 ||
                    gore > 0.5 ||
                    drugs > 0.5
                ) {
                    return res.status(422).json({
                        error: "La imagen contiene contenido inapropiado y no cumple con las normas de ArteNauta.",
                    });
                }
            } catch (errSight) {
                console.error(
                    "Error conectando con Sightengine:",
                    errSight.response?.data || errSight.message,
                );
                return res
                    .status(500)
                    .json({
                        error: "No se pudo verificar la seguridad de la imagen.",
                    });
            }
        }
        const { data, error } = await supabase.from("publicaciones").insert([
            {
                titulo,
                contenido,
                descripcion,
                fecha_publicacion: new Date().toISOString(),
                id_usuario_artista,
                id_categoria,
            },
        ]).select(`
                *,
                usuarios (
                    id_usuario,
                    nombre,
                    email
                )
            `);

        if (error) throw error;

        if (!data || data.length === 0) {
            throw new Error("No se pudo obtener la publicación creada.");
        }
        res.status(201).json(data[0]);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// DELETE: Eliminar una publicación
router.delete("/publicaciones/:id", verificarToken, async (req, res) => {
    try {
        const { id } = req.params;
        const { error } = await supabase
            .from("publicaciones")
            .delete()
            .eq("id_publicacion", id);

        if (error) throw error;

        res.status(200).json({
            message: "Publicación eliminada correctamente",
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Get: publicaciones del perfil
router.get("/mis-publicaciones/:id_usuario", async (req, res) => {
    try {
        const { id_usuario } = req.params;
        const { data, error } = await supabase
            .from("publicaciones")
            .select("*")
            .eq("id_usuario_artista", id_usuario)
            .order("id_publicacion", { ascending: false });

        if (error) throw error;
        res.json(data);
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});

// Edita título y descripción de una publicación
router.put("/publicaciones/:id_publicacion", async (req, res) => {
    try {
        const { id_publicacion } = req.params;
        const { titulo, descripcion } = req.body;

        const { data, error } = await supabase
            .from("publicaciones")
            .update({ titulo, descripcion })
            .eq("id_publicacion", id_publicacion)
            .select();

        if (error) throw error;
        res.json({
            message: "Publicación editada correctamente",
            data: data[0],
        });
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});

// DELETE: eliminar publicaciones
router.delete("/publicaciones/:id_publicacion", async (req, res) => {
    try {
        const { id_publicacion } = req.params;

        const { error } = await supabase
            .from("publicaciones")
            .delete()
            .eq("id_publicacion", id_publicacion);

        if (error) throw error;
        res.json({ message: "Publicación eliminada con éxito" });
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});

export default router;



================================================
FILE: backend/routes/reacciones.js
================================================
import express from "express";
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();


// POST: Agregar o retirar like de una publicacion
router.post(
  "/publicaciones/:id_publicacion/like",
  verificarToken,
  async (req, res) => {

    const { id_publicacion } = req.params;
    const { id_usuario } = req.body;

    const tipoReaccion = "like";

    if (!id_usuario) {
      return res.status(400).json({
        error: "El id_usuario es requerido."
      });
    }

    try {

      const { data: existente, error: errorBusqueda } = await supabase
        .from("reacciones")
        .select("*")
        .eq("id_publicacion", id_publicacion)
        .eq("id_usuario", id_usuario)
        .eq("tipo", tipoReaccion);

      if (errorBusqueda) throw errorBusqueda;

      if (existente.length > 0) {

        const { error: errorDelete } = await supabase
          .from("reacciones")
          .delete()
          .eq("id_publicacion", id_publicacion)
          .eq("id_usuario", id_usuario)
          .eq("tipo", tipoReaccion);

        if (errorDelete) throw errorDelete;

        return res.json({
          registrado: false,
          mensaje: "Like retirado con éxito"
        });
      }

      const { error: errorInsert } = await supabase
        .from("reacciones")
        .insert([
          {
            fecha: new Date(),
            tipo: tipoReaccion,
            id_usuario,
            id_publicacion
          }
        ]);

      if (errorInsert) throw errorInsert;

      return res.json({
        registrado: true,
        mensaje: "Like registrado con éxito"
      });

    } catch (error) {
      console.error("Error en POST /like:", error);

      return res.status(500).json({
        error: "Error interno al procesar el me gusta."
      });
    }
  }
);


// GET: Obtener información de likes
router.get(
  "/publicaciones/:id_publicacion/likes-info",
  async (req, res) => {

    const { id_publicacion } = req.params;
    const { id_usuario } = req.query;

    const tipoReaccion = "like";

    try {

      const { data: likes, error: errorLikes } = await supabase
        .from("reacciones")
        .select("*")
        .eq("id_publicacion", id_publicacion)
        .eq("tipo", tipoReaccion);

      if (errorLikes) throw errorLikes;

      const totalLikes = likes.length;

      let usuarioDioLike = false;

      if (id_usuario) {
        usuarioDioLike = likes.some(
          reaccion => reaccion.id_usuario == id_usuario
        );
      }

      return res.json({
        totalLikes,
        usuarioDioLike
      });

    } catch (error) {
      console.error("Error en GET /likes-info:", error);

      return res.status(500).json({
        error: "Error al obtener estadísticas de reacciones."
      });
    }
  }
);

export default router;


================================================
FILE: backend/routes/usuarios.js
================================================
import express from "express";
import bcrypt from 'bcrypt';
import { supabase } from "../config/supabase.js";
import { verificarToken } from "../middlewares/verificarToken.js";

const router = express.Router();

// PETICION POST REGISTRO DE USUARIOS
router.post('/usuario/registro', async (req, res) => {
    const { nombre, apellido, telefono, email, clave, id_rol } = req.body;

    try {
        const saltRounds = 10;
        const hashedPassword = await bcrypt.hash(clave, saltRounds);
        const { data, error } = await supabase
            .from('usuarios')
            .insert([
                {
                    nombre: nombre,
                    apellido: apellido,
                    telefono: telefono,
                    email: email,
                    clave: hashedPassword,
                    id_rol: id_rol
                }
            ]);

        if (error) throw error;

        res.status(201).json({ mensaje: 'Usuario registrado exitosamente' });

    } catch (error) {
        console.error("Error en el registro del backend:", error);
        res.status(500).json({ error: error.message || 'Hubo un error al registrar el usuario' });
    }
});

// PETICIÓN QUERY USUARIOS: ESTADO, BÚSQUEDA Y ROL
router.get("/usuarios", verificarToken, async (req, res) => {
    try {
        const { estado, buscar, rol } = req.query;

        let consulta = supabase
            .from("usuarios")
            .select(`id_usuario, nombre, apellido, email, estado_cuenta, id_rol, roles!id_rol (id_rol, nombre_rol), fecha_registro`)
            .order('id_usuario', { ascending: true });

        if (estado === "true") {
            consulta = consulta.eq("estado_cuenta", true);
        } else if (estado === "false") {
            consulta = consulta.eq("estado_cuenta", false);
        }

        if (buscar && buscar.trim() !== "") {
            consulta = consulta.or(`nombre.ilike.%${buscar}%,apellido.ilike.%${buscar}%,email.ilike.%${buscar}%`);
        }

        if (rol && rol.trim() !== "") {
            consulta = consulta.eq("id_rol", rol); 
        }

        const { data, error } = await consulta;

        if (error) throw error;

        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// PETICIÓN PATCH USUARIOS
router.patch("/usuarios/:id", verificarToken, async (req, res) => {
    try {
        const { id } = req.params;
        const { nombre, apellido, estado_cuenta, id_rol } = req.body;

        const { data, error } = await supabase
            .from("usuarios")
            .update({ 
                nombre, 
                apellido, 
                estado_cuenta, 
                id_rol: parseInt(id_rol)
            })
            .eq("id_usuario", id)
            .select(`id_usuario, nombre, apellido, email, estado_cuenta, id_rol, roles!id_rol (nombre_rol), fecha_registro`); 

        if (error) throw error;

        res.status(200).json(data[0]);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

export default router;


================================================
FILE: src/App.css
================================================
[Empty file]


================================================
FILE: src/App.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";
import Categorias from "./components/Categorias";
import Footer from "./components/Footer";
import Header from "./components/Header";
import Hero from "./components/Hero";
import Contacto from "./components/Contacto";
import PanelAdmin from "./components/paneles/PanelAdmin";
import PanelArtista from "./components/paneles/PanelArtista";
import PanelUsuario from "./components/paneles/PanelUsuario";
import RestablecerClave from "./components/RestablecerClave";
import { supabase } from "./lib/supabase";
import Preloader from "./components/Preloader";

function App() {
    const isRestablecer = window.location.pathname === "/restablecer-clave";
    const [showPreloader, setShowPreloader] = useState(!isRestablecer);
    const [pagina, setPagina] = useState("home");
    const [vista, setVista] = useState(() => {
        const usuarioGuardado = localStorage.getItem("usuario");
        const token = localStorage.getItem("token");
        if (!usuarioGuardado || !token) return null;
        try {
            const usuario = JSON.parse(usuarioGuardado);
            return usuario.id_rol; // Retorna 1, 2 o 3
        } catch (error) {
            console.log(error);
            return null;
        }
    });
    const cerrarSesionForzada = () => {
        localStorage.removeItem("token");
        localStorage.removeItem("usuario");
        
        setVista(null);
        
        Swal.fire({
            icon: "warning",
            title: "Sesión Expirada",
            text: "Tu tiempo de sesión ha terminado por seguridad.",
            confirmButtonColor: "#0891b2"
        }).then(() => {
            window.location.reload();
        });
    };
    useEffect(() => {
        const token = localStorage.getItem("token");

        if (token) {
            try {
                const payload = JSON.parse(atob(token.split(".")[1]));
                
                const tiempoExpiracion = payload.exp * 1000; 
                const tiempoRestante = tiempoExpiracion - Date.now();

                if (tiempoRestante <= 0) {
                    setTimeout(() => cerrarSesionForzada(), 0);
                } else {
                    const temporizador = setTimeout(() => {
                        cerrarSesionForzada();
                    }, tiempoRestante);

                    return () => clearTimeout(temporizador);
                }
            } catch (error) {
                console.error("Error al decodificar el token", error);
                setTimeout(() => cerrarSesionForzada(), 0);
            }
        }
    }, [vista]);

    useEffect(() => {
        const usuarioGuardado = localStorage.getItem("usuario");
        if (!usuarioGuardado) return;
        const usuario = JSON.parse(usuarioGuardado);
        if (!usuario?.id_usuario) return;

        const canal = supabase
            .channel(`rol-usuario-${usuario.id_usuario}`)
            .on("postgres_changes", {
                event: "UPDATE",
                schema: "public",
                table: "usuarios",
                filter: `id_usuario=eq.${usuario.id_usuario}`,
            }, (payload) => {
                const nuevoRol = payload.new?.id_rol;
                if (nuevoRol && nuevoRol !== usuario.id_rol) {
                    const actualizado = { ...usuario, id_rol: nuevoRol };
                    localStorage.setItem("usuario", JSON.stringify(actualizado));
                    setVista(nuevoRol);
                }
            })
            .subscribe();

        return () => supabase.removeChannel(canal);
    }, []);

    if (isRestablecer) return <RestablecerClave />;

    if (showPreloader) return <Preloader onComplete={() => setShowPreloader(false)} />;

    if (vista === 3) return <PanelAdmin setVista={setVista} />;
    if (vista === 2) return <PanelArtista setVista={setVista} />;
    if (vista === 1) return <PanelUsuario setVista={setVista} />;

    return (
        <div className="flex flex-col min-h-screen">
            <Header setPagina={setPagina} setVista={setVista} />
            <Hero pagina={pagina} />
            <main className="flex-1">
                {pagina === "home" && <Categorias />}
                {pagina === "contacto" && <Contacto />}
            </main>
            <Footer />
        </div>
    );
}

export default App;


================================================
FILE: src/index.css
================================================
@keyframes fadeInBackdrop {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}

.animar-fondo {
    animation: fadeInBackdrop 0.3s ease-out forwards;
}

@keyframes popIn {
    from {
        opacity: 0;
        transform: scale(0.90);
    }

    to {
        opacity: 1;
        transform: scale(1);
    }
}

.animar-pop {
    animation: popIn 0.3s ease-out forwards;
}

/* Animación para ocultar el fondo oscuro */
@keyframes fadeOutBackdrop {
    from {
        opacity: 1;
    }

    to {
        opacity: 0;
    }
}

.animar-fondo-salida {
    animation: fadeOutBackdrop 0.3s ease-in forwards;
}

/* Animación para que la ventana se encoja y desaparezca */
@keyframes popOut {
    from {
        opacity: 1;
        transform: scale(1);
    }

    to {
        opacity: 0;
        transform: scale(0.90);
    }
}

.animar-pop-salida {
    animation: popOut 0.3s ease-in forwards;
}


================================================
FILE: src/main.jsx
================================================
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import "./index.css";
import App from "./App.jsx";

createRoot(document.getElementById("root")).render(
    <StrictMode>
        <App />
    </StrictMode>,
);



================================================
FILE: src/components/BotonLike.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";

export default function BotonLike({
    idPublicacion,
    idUsuarioActual,
    className = "",
    showText = true,
    iconSize = "16"
}) {

    const [totalLikes, setTotalLikes] = useState(0);
    const [dioLike, setDioLike] = useState(false);
    const [cargando, setCargando] = useState(true);

    useEffect(() => {

        const obtenerDatosLikes = async () => {
            try {

                const res = await fetch(
                    `http://localhost:3000/publicaciones/${idPublicacion}/likes-info?id_usuario=${idUsuarioActual || ""}`
                );

                if (!res.ok) {
                    throw new Error("No se pudieron obtener los likes.");
                }

                const data = await res.json();

                setTotalLikes(data.totalLikes ?? 0);
                setDioLike(data.usuarioDioLike ?? false);

            } catch (error) {
                console.error("Error al traer los likes:", error);
            } finally {
                setCargando(false);
            }
        };

        if (idPublicacion) {
            obtenerDatosLikes();
        }

    }, [idPublicacion, idUsuarioActual]);

    const handleLikeClick = async (e) => {
        e.stopPropagation();

        if (!idUsuarioActual) {
            Swal.fire({
                icon: "warning",
                title: "Sesión requerida",
                text: "Debes iniciar sesión para dar me gusta.",
                confirmButtonColor: "#0891b2"
            });
            return;
        }

        const estadoPrevioLike = dioLike;
        const contadorPrevio = totalLikes;

        setDioLike(!dioLike);
        setTotalLikes(prev => dioLike ? prev - 1 : prev + 1);

        try {

            const token = localStorage.getItem("token");

            const res = await fetch(
                `http://localhost:3000/publicaciones/${idPublicacion}/like`,
                {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                        ...(token && {
                            Authorization: `Bearer ${token}`
                        })
                    },
                    body: JSON.stringify({
                        id_usuario: idUsuarioActual
                    })
                }
            );

            const data = await res.json();

            if (!res.ok) {
                throw new Error(
                    data.error || "No se pudo procesar el like."
                );
            }

        } catch (error) {

            console.error("Error al procesar el like:", error);
            setDioLike(estadoPrevioLike);
            setTotalLikes(contadorPrevio);

            Swal.fire({
                icon: "error",
                title: "Error",
                text: error.message,
                confirmButtonColor: "#0891b2"
            });
        }
    };

    if (cargando) {
        return (
            <span className="text-xs text-gray-400">
                ...
            </span>
        );
    }

    const estiloBoton = className || `
        flex items-center gap-1.5 px-3 py-1.5 rounded-full
        border transition text-xs font-medium
        ${
            dioLike
                ? "border-red-200 bg-red-50 text-red-500"
                : "border-gray-100 text-gray-400 hover:border-red-200 hover:text-red-400"
        }
    `;

    return (
        <button
            onClick={handleLikeClick}
            className={estiloBoton}
        >
            <svg
                width={iconSize}
                height={iconSize}
                viewBox="0 0 24 24"
                fill={dioLike ? "currentColor" : "none"}
                stroke="currentColor"
                strokeWidth="2"
            >
                <path d="M12 21C12 21 3 15.5 3 9.5C3 7 5 5 7.5 5C9.5 5 11 6.5 12 8C13 6.5 14.5 5 16.5 5C19 5 21 7 21 9.5C21 15.5 12 21 12 21Z" />
            </svg>

            {showText && (
                <span>
                    {totalLikes}
                </span>
            )}
        </button>
    );
}


================================================
FILE: src/components/Categorias.jsx
================================================
import categoria1 from "../assets/categoria1.jpg"
import categoria2 from "../assets/categoria2.jpg"
import categoria3 from "../assets/categoria3.jpg"
import categoria4 from "../assets/categoria4.jpg"
import categoria5 from "../assets/categoria5.jpg"
import categoria6 from "../assets/categoria6.jpg"
function Categorias(){
    return(
        <section className="py-6 max-w-7xl mx-auto">
            <h2 className="text-cyan-600 text-3xl font-bold mb-6 text-center">Categorias</h2>
            <div className="grid grid-cols-3 gap-6">
                <div className="bg-white rounded-2xl shadow p-4 cursor-pointer hover:shadow-md transition relative flex flex-col h-full justify-between">
                    <img src={categoria1} alt="Artes Plasticas" className="w-full h-52 object-cover"/>
                    <h3 className="py-3 font-medium text-center">Artes Plasticas</h3>
                    <p className="text-center px-2">Pinturas, Esculturas, Dibujo, Grabado, etc.</p>
                </div>
                <div className="bg-white rounded-2xl shadow p-4 cursor-pointer hover:shadow-md transition relative flex flex-col h-full justify-between">
                    <img src={categoria2} alt="Artes Musicales" className="w-full h-52 object-cover"/>
                    <h3 className="py-3 font-medium text-center">Artes Musicales</h3>
                    <p className="text-center px-2">Trabajos musicales independientes, Composiciones y Construccion de samples.</p>
                </div>
                <div className="bg-white rounded-2xl shadow p-4 cursor-pointer hover:shadow-md transition relative flex flex-col h-full justify-between">
                    <img src={categoria3} alt="Artes Danzarias" className="w-full h-52 object-cover"/>
                    <h3 className="py-3 font-medium text-center">Artes Danzarias</h3>
                    <p className="text-center px-2">Coreografias basadas en hip hop, breaking, locking, popping, house, dancehall y waacking.</p>
                </div>
                <div className="bg-white rounded-2xl shadow p-4 cursor-pointer hover:shadow-md transition relative flex flex-col h-full justify-between">
                    <img src={categoria4} alt="Artes Musicales" className="w-full h-52 object-cover"/>
                    <h3 className="py-3 font-medium text-center">Artes Urbanas</h3>
                    <p className="text-center px-2">Trabajos que toman el espacio público como lienzo.</p>
                </div>
                <div className="bg-white rounded-2xl shadow p-4 cursor-pointer hover:shadow-md transition relative flex flex-col h-full justify-between">
                    <img src={categoria5} alt="Artes Musicales" className="w-full h-52 object-cover"/>
                    <h3 className="py-3 font-medium text-center">Artes Digitales y Multimedia</h3>
                    <p className="text-center px-2">Trabajos Utilizan herramientas informáticas, software y dispositivos digitales como el medio principal de creación y difusión.</p>
                </div>
                <div className="bg-white rounded-2xl shadow p-4 cursor-pointer hover:shadow-md transition relative flex flex-col h-full justify-between">
                    <img src={categoria6} alt="Artes Musicales" className="w-full h-52 object-cover"/>
                    <h3 className="py-3 font-medium text-center">Artes Audiovisuales</h3>
                    <p className="text-center px-2">Es la categoría que combina el sonido y la vista de manera sincrónica para crear experiencias narrativas o expresivas.</p>
                </div>
            </div>
        </section>
    )
}

export default Categorias


================================================
FILE: src/components/Contacto.jsx
================================================
import logoWhatsapp from "../assets/logo-whatsapp.svg"
import logoGmail from "../assets/gmail.png"
import logoMaps from "../assets/maps.png"

function Contacto(){
    return(
        <section className="max-w-4xl mx-auto my-4 bg-white rounded-2xl overflow-hidden shadow-md flex flex-col md:flex-row">
            <div className="bg-gradient-to-b from-cyan-900 to-cyan-600 text-white p-10 flex flex-col gap-6 md:w-2/5">
                <div>
                    <h2 className="text-xl font-medium mb-2">Contáctanos</h2>
                    <p className="text-sm text-cyan-200">Llena el formulario o usa cualquiera de nuestros canales de contacto.</p>
                </div>
                <div className="flex flex-col gap-4">
                    <div className="flex items-center gap-3">
                        <div className="bg-white/20 p-2 rounded-lg">
                            <img src={logoGmail} alt="logo de gmail" className="w-5"/>
                        </div>
                        <div>
                            <p className="text-xs text-cyan-300 uppercase tracking-wide">Correo</p>
                            <p className="text-sm">ArteNauta@gmail.com</p>
                        </div>
                    </div>
                    <div className="flex items-center gap-3">
                        <div className="bg-white/20 p-2 rounded-lg">
                            <img src={logoWhatsapp} alt="logo de whatsapp" className="w-5"/>
                        </div>
                        <div>
                            <p className="text-xs text-cyan-300 uppercase tracking-wide">Whatsapp</p>
                            <p className="text-sm">3118692159</p>
                        </div>
                    </div>
                    <div className="flex items-center gap-3">
                        <div className="bg-white/20 p-2 rounded-lg">
                            <img src={logoMaps} alt="logo de maps" className="w-5"/>
                        </div>
                        <div>
                            <p className="text-xs text-cyan-300 uppercase tracking-wide">Ubicación</p>
                            <p className="text-sm">Colombia</p>
                        </div>
                    </div>
                </div>
            </div>
            <form action="" className="p-5 flex flex-col gap-4 flex-1">
                <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                        Nombre
                    </label>
                    <input type="text" placeholder="Nombre Completo" className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500"/>
                </div>
                <div>
                    <label className="block mt-3 text-sm font-medium text-gray-700 mb-1">
                        Email
                    </label>
                    <input type="email" placeholder="Correo Electronico" className="text-black w-full border border-gray-300 rounded-lg px-4 py-2    focus:outline-none focus:ring-2 focus:ring-cyan-500"/>
                </div>
                <div>
                    <label className="block mt-3 text-sm font-medium text-gray-700 mb-1">
                        Mensaje
                    </label>
                    <textarea placeholder="Mensaje" className="text-black w-full border border-gray-300 rounded-lg px-4 py-2    focus:outline-none focus:ring-2 focus:ring-cyan-500" rows="10"/>
                </div>
                <div className="mt-3 text-center">
                    <button className="bg-cyan-600 text-white box-border p-2 rounded px-8 mx-1 hover:bg-cyan-900">Envíar</button>
                </div>
            </form>
        </section>
    )
}

export default Contacto


================================================
FILE: src/components/Footer.jsx
================================================
function Footer(){
    return(
        <footer className="bg-gradient-to-t from-cyan-700 to-cyan-900 text-white text-center py-5 mt-auto">
            <p className="text-lg font-medium">
                &copy;2026 - ArteNauta
            </p>
        </footer>
    )
}

export default Footer


================================================
FILE: src/components/FormPublicaciones.jsx
================================================
import { useState, useEffect } from "react";
import axios from "axios";
import Swal from "sweetalert2";

export default function FormPublicaciones({
    onNuevaPublicacion,
    onClose,
    idArtistaActivo
}) {
    const [form, setForm] = useState({
        Titulo: "",
        Descripcion: "",
        contenido: "",
        id_categoria: "",
    });

    const [categorias, setCategorias] = useState([]);
    const [loadingCategorias, setLoadingCategorias] = useState(true);
    const [loading, setLoading] = useState(false);
    const [isClosing, setIsClosing] = useState(false);

    useEffect(() => {
        const obtenerCategorias = async () => {
            try {
                const token = localStorage.getItem("token");
                const res = await fetch("http://localhost:3000/categorias", {
                    method: "GET",
                    headers: {
                        "Content-Type": "application/json",
                        ...(token && { "Authorization": `Bearer ${token}` })
                    }
                });

                if (!res.ok) throw new Error("Error al cargar las categorías fijas.");

                const data = await res.json();
                const lista = data.data ? data.data : data;
                setCategorias(Array.isArray(lista) ? lista : []);
            } catch (err) {
                console.error("Error cargando categorías:", err);
            } finally {
                setLoadingCategorias(false);
            }
        };

        obtenerCategorias();
    }, []);
    const manejarCierre = () => {
        setIsClosing(true);
        setTimeout(() => {
            onClose();
        }, 250);
    };

    const handleChange = (e) => {
        setForm({
            ...form,
            [e.target.name]: e.target.value,
        });
    };

    const handleSubmit = async () => {
        const token = localStorage.getItem("token");

        if (!form.Titulo.trim() || !form.Descripcion.trim()) {
            Swal.fire({
                icon: "warning",
                title: "Campos incompletos",
                text: "El título y la descripción son obligatorios.",
                confirmButtonColor: "#0891b2"
            });
            return;
        }

        if (!form.id_categoria) {
            Swal.fire({
                icon: "warning",
                title: "Falta categoría",
                text: "Por favor, selecciona una categoría para tu obra.",
                confirmButtonColor: "#0891b2"
            });
            return;
        }

        setLoading(true);

        try {
            const usuarioSesion = JSON.parse(localStorage.getItem('usuario'));
            const artistaId = idArtistaActivo || usuarioSesion?.id_usuario || usuarioSesion?.id;

            const payload = {
                titulo: form.Titulo,
                descripcion: form.Descripcion,
                contenido: form.contenido,
                id_usuario_artista: artistaId,
                id_categoria: Number(form.id_categoria)
            };

            const res = await fetch("http://localhost:3000/publicaciones", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": `Bearer ${token}`,
                },
                body: JSON.stringify(payload),
            });

            if (!res.ok) {
                const errData = await res.json();
                throw new Error(errData.error || "No se pudo crear la publicación.");
            }

            const nuevaPub = await res.json();
            Swal.fire({
                icon: "success",
                title: "¡Obra Publicada!",
                text: "Tu publicación se ha creado exitosamente en ArteNauta.",
                timer: 2000,
                showConfirmButton: false
            });

            onNuevaPublicacion(nuevaPub);
            setTimeout(() => {
                manejarCierre();
            }, 500);

        } catch (err) {
            console.error(err);
            Swal.fire({
                icon: "error",
                title: "Contenido Rechazado",
                text: err.message,
                confirmButtonColor: "#0891b2"
            });
        } finally {
            setLoading(false);
        }
    };

    return (
        <div
            className="fixed inset-0 z-50 flex items-center justify-center backdrop-blur-sm animar-fondo"
            style={{ backgroundColor: "rgba(0,0,0,0.5)" }}
            onClick={manejarCierre}
        >
            <div
                className={`bg-white rounded-2xl shadow-xl w-full max-w-md p-8 relative ${
                    isClosing ? "animar-pop-salida" : "animar-pop"
                }`}
                onClick={(e) => e.stopPropagation()}
            >
                {/* BOTÓN CIERRE */}
                <button 
                    onClick={manejarCierre} 
                    className="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-2xl"
                >
                    ✕
                </button>

                {/* HEADER */}
                <div className="flex justify-between items-center mb-5">
                    <h2 className="text-xl font-semibold text-gray-700">
                        Nueva publicación
                    </h2>
                </div>

                {/* FORM */}
                <div className="flex flex-col gap-3">
                    {/* TITULO */}
                    <input
                        type="text"
                        name="Titulo"
                        value={form.Titulo}
                        onChange={handleChange}
                        placeholder="Título"
                        className="border border-gray-200 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400"
                    />

                    {/* DESCRIPCION */}
                    <textarea
                        name="Descripcion"
                        value={form.Descripcion}
                        onChange={handleChange}
                        placeholder="Descripción"
                        rows={3}
                        className="border border-gray-200 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400 resize-none"
                    />

                    {/* SELECT CATEGORÍAS */}
                    <select
                        name="id_categoria"
                        value={form.id_categoria}
                        onChange={handleChange}
                        className="border border-gray-200 rounded-xl px-4 py-2 text-sm bg-white text-gray-700 focus:outline-none focus:ring-2 focus:ring-cyan-400 cursor-pointer"
                        disabled={loadingCategorias}
                    >
                        <option value="" disabled>
                            {loadingCategorias ? "Cargando categorías..." : "Selecciona una categoría"}
                        </option>
                        {categorias.map((cat) => (
                            <option key={cat.id_categoria} value={cat.id_categoria}>
                                {cat.nombre_categoria}
                            </option>
                        ))}
                    </select>

                    {/* URL IMAGEN */}
                    <input
                        type="text"
                        name="contenido"
                        value={form.contenido}
                        onChange={handleChange}
                        placeholder="URL de la imagen"
                        className="border border-gray-200 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400"
                    />

                    {/* PREVIEW */}
                    {form.contenido && (
                        <div className="w-full h-48 rounded-xl overflow-hidden bg-gray-100 flex items-center justify-center">
                            <img
                                src={form.contenido}
                                alt="Preview"
                                className="w-full h-full object-contain"
                                onError={(e) => {
                                    e.target.style.display = "none";
                                }}
                            />
                        </div>
                    )}

                    {/* BOTON ENVIAR */}
                    <button
                        onClick={handleSubmit}
                        disabled={loading}
                        className="bg-cyan-600 text-white rounded-xl py-2 mt-2 text-sm font-medium hover:bg-cyan-700 transition disabled:opacity-50"
                    >
                        {loading ? "Validando y publicando..." : "Publicar"}
                    </button>
                </div>
            </div>
        </div>
    );
}


================================================
FILE: src/components/Header.jsx
================================================
import { useState, useEffect } from "react";
import InicioSesionModal from "../components/InicioSesionModal";
import RegistroModal from "../components/RegistroModal";

function Header({ setPagina, setVista }) {
    const [showLogin, setShowLogin] = useState(false);
    const [showRegistro, setShowRegistro] = useState(false);
    const [animate, setAnimate] = useState(false);

    useEffect(() => {
        const timer = setTimeout(() => setAnimate(true), 50);
        return () => clearTimeout(timer);
    }, []);

    const cambiarRegistroALogin = () => {
        setShowRegistro(false);
        setShowLogin(true);
    };

    const cambiarLoginARegistro = () => {
        setShowLogin(false);
        setShowRegistro(true);
    };

    return (
        <header className="bg-gradient-to-r from-cyan-900 to-cyan-400 text-white p-4 flex justify-between items-center relative">
            <div
                className={`transition-all duration-[1000ms] ease-out ${animate ? "translate-x-0 translate-y-0 opacity-100" : "-translate-x-6 -translate-y-2 opacity-0"}`}
            >
                <img src="../src/assets/LOGO.png" alt="Logo" className="h-20" />
            </div>
            <nav
                className={`transition-all duration-[1000ms] ease-out delay-150 ${animate ? "translate-x-0 translate-y-0 opacity-100" : "translate-x-6 -translate-y-2 opacity-0"}`}
            >
                <ul>
                    <button
                        onClick={() => setPagina("home")}
                        className="bg-cyan-600 box-border p-2 rounded mx-1 hover:bg-cyan-900 transition-colors duration-300 cursor-pointer"
                    >
                        Inicio
                    </button>
                    <button
                        onClick={() => setPagina("contacto")}
                        className="bg-cyan-600 box-border p-2 rounded mx-1 hover:bg-cyan-900 transition-colors duration-300 cursor-pointer"
                    >
                        Contáctanos
                    </button>
                    <button
                        onClick={() => setShowRegistro(true)}
                        className="bg-cyan-600 box-border p-2 rounded mx-1 hover:bg-cyan-900 transition-colors duration-300 cursor-pointer"
                    >
                        Regístrate
                    </button>
                    <button
                        onClick={() => setShowLogin(true)}
                        className="bg-cyan-600 box-border p-2 rounded mx-1 hover:bg-cyan-900 transition-colors duration-300 cursor-pointer"
                    >
                        Iniciar Sesión
                    </button>
                </ul>
            </nav>

            <InicioSesionModal
                isOpen={showLogin}
                onClose={() => setShowLogin(false)}
                setVista={setVista}
                alCambiarAModalRegistro={cambiarLoginARegistro}
            />

            <RegistroModal
                isOpen={showRegistro}
                onClose={() => setShowRegistro(false)}
                alCambiarAModalLogin={cambiarRegistroALogin}
            />
        </header>
    );
}

export default Header;



================================================
FILE: src/components/Hero.jsx
================================================
import { useState, useEffect } from "react";

function Hero({pagina}){
    const [animate, setAnimate] = useState(false);

    useEffect(() => {
        const timer = setTimeout(() => setAnimate(true), 50);
        return () => clearTimeout(timer);
    }, []);

    let titulo, descripcion
    if (pagina === "home") {
        titulo = "El hogar de los artistas y quienes los admiran"
        descripcion = "Explora publicaciones, reacciona, comenta y chatea directamente con artistas. Una red social hecha para el arte."
    } else if (pagina === "contacto") {
        titulo = "¿Tienes alguna pregunta?"
        descripcion = "¿Dudas, sugerencias o simplemente quieres saludarnos? Escríbenos, leemos todos los mensajes."
    } else {
        titulo = "El hogar de los artistas y quienes los admiran"
        descripcion = "Explora publicaciones, reacciona, comenta y chatea directamente con artistas. Una red social hecha para el arte."
    }
    return(
        <section className="bg-gradient-to-l from-cyan-900 to-cyan-400 text-white text-center pt-8 px-100 overflow-hidden">
            <h1 className={`text-3xl font-semibold mb-4 transition-all duration-[1000ms] ease-out ${animate ? 'translate-y-0 opacity-100' : 'translate-y-6 opacity-0'}`}>{titulo}</h1>
            <p className={`text-lg mb-6 transition-all duration-[1200ms] ease-out delay-200 ${animate ? 'translate-y-0 opacity-100' : 'translate-y-4 opacity-0'}`}>{descripcion}</p>
        </section>
    )
}

export default Hero


================================================
FILE: src/components/InicioSesionModal.jsx
================================================
import { useState } from "react";
import Swal from "sweetalert2";

function LoginModal({ isOpen, onClose, setVista, alCambiarAModalRegistro }) {
    const [isClosing, setIsClosing] = useState(false);
    const [correo, setCorreo] = useState("");
    const [password, setPassword] = useState("");
    const [vistaActual, setVistaActual] = useState("login"); // "login" o "recuperar"
    const [cargando, setCargando] = useState(false);
    
    if (!isOpen && !isClosing) return null;

    const manejarCierre = () => {
        setIsClosing(true); 
        setTimeout(() => {
            onClose(); 
            setIsClosing(false); 
        }, 300); 
    };

    const manejarLogin = async (e) => {
        e.preventDefault();

        try {
            const respuesta = await fetch("http://localhost:3000/auth/login", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ email: correo, password: password }),
            });

            const data = await respuesta.json();

            if (respuesta.ok) {
                localStorage.setItem("usuario", JSON.stringify(data.usuario));
                localStorage.setItem("token", data.token);
                
                onClose();
                setVista(data.usuario.id_rol);
                
                Swal.fire({
                    icon: "success",
                    title: "¡Bienvenido de nuevo!",
                    text: "Inicio de sesión exitoso",
                    confirmButtonColor: "#0891b2",
                    background: "#ecfeff",
                    color: "#164e63",
                    timer: 2000,
                    showConfirmButton: false,
                });
            } else {
                console.log(data.mensaje);
                Swal.fire({
                    icon: "error",
                    title: "Ups...",
                    text: data.mensaje || "Ocurrió un error al iniciar sesión",
                    confirmButtonColor: "#0891b2",
                    confirmButtonText: "Intentar de nuevo",
                });
            }
        } catch (error) {
            console.log(error);
            Swal.fire({
                icon: "error",
                title: "Error de conexión",
                text: "No es posible conectarse al servidor",
                confirmButtonColor: "#0891b2",
            });
        }
    };

    const manejarRecuperacion = async (e) => {
        e.preventDefault();
        setCargando(true);

        try {
            const respuesta = await fetch("http://localhost:3000/auth/forgot-password", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ email: correo }),
            });

            const data = await respuesta.json();

            if (respuesta.ok) {
                Swal.fire({
                    icon: "success",
                    title: "Correo enviado",
                    text: data.mensaje,
                    confirmButtonColor: "#0891b2",
                });
                setVistaActual("login");
            } else {
                Swal.fire({
                    icon: "error",
                    title: "Ups...",
                    text: data.mensaje || "Error al solicitar la recuperación",
                    confirmButtonColor: "#0891b2",
                });
            }
        } catch (error) {
            console.log(error);
            Swal.fire({
                icon: "error",
                title: "Error de conexión",
                text: "No es posible conectarse al servidor",
                confirmButtonColor: "#0891b2",
            });
        } finally {
            setCargando(false);
        }
    };

    return (
        <div className="fixed inset-0 z-50 flex items-center justify-center backdrop-blur-sm animar-fondo" style={{ backgroundColor: "rgba(0,0,0,0.5)" }} onClick={onClose}>
            <div className={`bg-white rounded-2xl shadow-xl w-full max-w-md p-8 relative ${isClosing ? "animar-pop-salida" : "animar-pop"}`} onClick={(e) => e.stopPropagation()}>
                <button onClick={manejarCierre} className="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-2xl animar-pop">
                    ✕
                </button>
                {vistaActual === "login" ? (
                    <>
                        <h2 className="text-2xl font-bold text-center text-cyan-600 mb-1">
                            Iniciar Sesión
                        </h2>
                        <p className="text-center text-gray-500 text-sm mb-6">
                            Bienvenido de nuevo a ArteNauta
                        </p>
                        <div className="flex flex-col gap-4">
                            <form onSubmit={manejarLogin}>
                                <div>
                                    <label className="block text-sm font-medium text-gray-700 mb-1">
                                        Email
                                    </label>
                                    <input type="email" placeholder="Correo Electronico" className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500" value={correo} onChange={(e) => setCorreo(e.target.value)} />
                                </div>
                                <div className="mt-4">
                                    <label className="block text-sm font-medium text-gray-700 mb-1">
                                        Contraseña
                                    </label>
                                    <input type="password" placeholder="Contraseña" className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500" value={password} onChange={(e) => setPassword(e.target.value)} required />
                                </div>
                                <p onClick={() => setVistaActual("recuperar")} className="text-right text-sm text-cyan-600 my-3 hover:underline cursor-pointer">
                                    ¿Olvidaste tu contraseña?
                                </p>
                                <button className="w-full bg-cyan-600 hover:bg-cyan-700 text-white font-semibold py-2 rounded-lg transition cursor-pointer">
                                    Entrar
                                </button>
                            </form>
                            <p className="text-center text-sm text-gray-500">
                                ¿No tienes cuenta? {" "}
                                <span onClick={alCambiarAModalRegistro} className="text-cyan-600 hover:underline cursor-pointer font-medium">
                                    Regístrate
                                </span>
                            </p>
                        </div>
                    </>
                ) : (
                    <>
                        <h2 className="text-2xl font-bold text-center text-cyan-600 mb-1">
                            Recuperar Contraseña
                        </h2>
                        <p className="text-center text-gray-500 text-sm mb-6">
                            Ingresa tu correo para recibir un enlace
                        </p>
                        <div className="flex flex-col gap-4">
                            <form onSubmit={manejarRecuperacion}>
                                <div>
                                    <label className="block text-sm font-medium text-gray-700 mb-1">
                                        Email
                                    </label>
                                    <input type="email" placeholder="Correo Electrónico" className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500" value={correo} onChange={(e) => setCorreo(e.target.value)} required />
                                </div>
                                <button disabled={cargando} className="w-full mt-6 bg-cyan-600 hover:bg-cyan-700 text-white font-semibold py-2 rounded-lg transition cursor-pointer disabled:opacity-50">
                                    {cargando ? "Enviando..." : "Enviar Enlace"}
                                </button>
                            </form>
                            <p className="text-center text-sm text-gray-500 mt-2">
                                <span onClick={() => setVistaActual("login")} className="text-cyan-600 hover:underline cursor-pointer font-medium">
                                    Volver al Login
                                </span>
                            </p>
                        </div>
                    </>
                )}
            </div>
        </div>
    );
}

export default LoginModal;


================================================
FILE: src/components/Preloader.jsx
================================================
import { useEffect, useState } from "react";
import logo from "../assets/LOGO.png";

function Preloader({ onComplete }) {
    const [mounted, setMounted] = useState(false);
    const [animateOut, setAnimateOut] = useState(false);

    useEffect(() => {
        // Trigger the entrance animations in the next frame
        const entranceTimer = setTimeout(() => {
            setMounted(true);
        }, 50);

        // Start the exit transition after the progress bar finishes loading (2.4 seconds)
        const exitTimer = setTimeout(() => {
            setAnimateOut(true);
        }, 2400);

        // Notify parent to unmount once the overlay transition finishes (3.2 seconds total)
        const completeTimer = setTimeout(() => {
            onComplete();
        }, 3200);

        return () => {
            clearTimeout(entranceTimer);
            clearTimeout(exitTimer);
            clearTimeout(completeTimer);
        };
    }, [onComplete]);

    return (
        <div 
            className={`fixed inset-0 z-50 flex flex-col items-center justify-center bg-black transition-all duration-[800ms] ease-[cubic-bezier(0.85,0,0.15,1)] ${
                animateOut ? "-translate-y-full opacity-0" : "translate-y-0 opacity-100"
            }`}
        >
            {/* Cinematic Radial background glow */}
            <div 
                className={`absolute inset-0 bg-[radial-gradient(circle_at_center,rgba(6,182,212,0.12)_0%,rgba(0,0,0,0)_75%)] pointer-events-none transition-opacity duration-[2000ms] ease-in-out ${
                    mounted ? "opacity-100" : "opacity-0"
                }`} 
            />
            
            {/* Centered Logo and Brand Info */}
            <div className="relative z-10 flex flex-col items-center select-none">
                <img 
                    src={logo} 
                    alt="ArteNauta Logo" 
                    className={`h-28 md:h-36 object-contain transition-all duration-[2200ms] ease-[cubic-bezier(0.25,1,0.5,1)] filter ${
                        animateOut 
                            ? "opacity-0 scale-105 blur-sm" 
                            : mounted 
                                ? "opacity-100 scale-100 blur-0 brightness-100 drop-shadow-[0_0_30px_rgba(6,182,212,0.45)]" 
                                : "opacity-0 scale-95 blur-md brightness-50 drop-shadow-[0_0_0px_rgba(6,182,212,0)]"
                    }`}
                />
                
                {/* Brand Name Text */}
                <h2 
                    className={`mt-4 text-white font-extralight tracking-[0.35em] text-xs md:text-sm uppercase transition-all duration-[2000ms] ease-out delay-200 ${
                        animateOut
                            ? "opacity-0 -translate-y-2 blur-[2px]"
                            : mounted 
                                ? "opacity-80 translate-y-0" 
                                : "opacity-0 translate-y-3"
                    }`}
                >
                    !Bienvenido¡
                </h2>
                
                {/* Sleek, minimal progress bar */}
                <div 
                    className={`mt-8 w-36 md:w-44 h-[2px] bg-neutral-900 rounded-full overflow-hidden transition-all duration-700 ${
                        animateOut ? "opacity-0 scale-95" : "opacity-100"
                    }`}
                >
                    <div 
                        className={`h-full bg-gradient-to-r from-cyan-500 to-cyan-300 rounded-full transition-all duration-[2100ms] ease-[cubic-bezier(0.65,0,0.35,1)] delay-100 ${
                            mounted ? "w-full" : "w-0"
                        }`} 
                    />
                </div>
            </div>
        </div>
    );
}

export default Preloader;



================================================
FILE: src/components/PublicacionesCom.jsx
================================================
import { useState } from "react";
import Swal from "sweetalert2";
import BotonLike from "./BotonLike"; 

export default function PublicacionesCom({ item }) {
    const [showDetalleModal, setShowDetalleModal] = useState(false);
    const [comentarios, setComentarios] = useState([]);
    const [loadingCom, setLoadingCom] = useState(false);
    const [errorCom, setErrorCom] = useState(null);
    const [nuevoComentario, setNuevoComentario] = useState("");
    const [submittingCom, setSubmittingCom] = useState(false);

    const usuarioGuardado = localStorage.getItem("usuario");
    const usuarioLogueado = usuarioGuardado ? JSON.parse(usuarioGuardado) : null;
    
    const tituloStr = item.titulo || item.Titulo || "Sin título";
    const descripcionStr = item.descripcion || item.Descripcion || "Sin descripción.";
    const contenidoUrl = item.contenido || item.Contenido || "";
    const autorNombre = item.usuarios?.nombre || "Artista";
    const autorAvatar = item.usuarios?.avatar_url || null;

    const postId = item.id_publicacion || item.id;
    const userId = usuarioLogueado?.id_usuario || usuarioLogueado?.id;
    const abrirDetalleModal = async () => {
        const token = localStorage.getItem("token");
        setShowDetalleModal(true);
        setLoadingCom(true);
        setErrorCom(null);

        try {
            const res = await fetch(`http://localhost:3000/comentarios/${postId}`, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
            });
            if (!res.ok) throw new Error("Error al obtener los comentarios desde el servidor.");
            const data = await res.json();
            setComentarios(data || []);
        } catch (err) {
            setErrorCom(err.message);
        } finally {
            setLoadingCom(false);
        }
    };

    const enviarComentario = async (e) => {
        const token = localStorage.getItem("token");
        e.preventDefault();
        if (!nuevoComentario.trim()) return;

        if (!usuarioLogueado) {
            Swal.fire({
                icon: "warning",
                title: "Sesión requerida",
                text: "Debes iniciar sesión para comentar en esta publicación.",
                confirmButtonColor: "#0891b2",
            });
            return;
        }

        setSubmittingCom(true);
        try {
            const res = await fetch("http://localhost:3000/comentarios", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
                body: JSON.stringify({
                    id_publicacion: postId,
                    id_usuario_final: userId,
                    contenido: nuevoComentario.trim(),
                }),
            });

            if (!res.ok) {
                const errData = await res.json();
                throw new Error(errData.error || "No se pudo publicar el comentario en el servidor.");
            }

            const nuevoComentarioCreado = await res.json();
            setComentarios((prev) => [...prev, nuevoComentarioCreado]);
            setNuevoComentario("");
        } catch (err) {
            console.error("Error al enviar comentario:", err);
            Swal.fire({
                icon: "error",
                title: "Error",
                text: "No se pudo publicar tu comentario: " + err.message,
                confirmButtonColor: "#0891b2",
            });
        } finally {
            setSubmittingCom(false);
        }
    };

    return (
        <>
            <div className="bg-white rounded-2xl shadow p-4 hover:shadow-md transition relative flex flex-col h-full justify-between">
                <div>
                    {/* Autor de la publicación */}
                    {item.usuarios && (
                        <div className="flex items-center gap-2 mb-3">
                            {autorAvatar ? (
                                <img
                                    src={autorAvatar}
                                    alt={autorNombre}
                                    className="w-7 h-7 rounded-full object-cover border border-cyan-100"
                                />
                            ) : (
                                <div className="w-7 h-7 rounded-full bg-cyan-100 flex items-center justify-center text-xs font-semibold text-cyan-700">
                                    {autorNombre?.charAt(0).toUpperCase()}
                                </div>
                            )}
                            <span className="text-xs font-semibold text-gray-500">
                                {autorNombre}
                            </span>
                        </div>
                    )}

                    {/* Imagen con Hover estilo Instagram */}
                    {contenidoUrl && (
                        <div 
                            onClick={abrirDetalleModal}
                            className="w-full h-48 rounded-lg mb-3 overflow-hidden bg-gray-100 cursor-pointer relative group"
                        >
                            <img
                                src={contenidoUrl}
                                alt={tituloStr}
                                className="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105"
                                onError={(e) =>
                                    (e.target.src = "https://placehold.co/300x200?text=Sin+imagen")
                                }
                            />
                            {/* Overlay de Hover estilo Instagram */}
                            <div className="absolute inset-0 bg-black/40 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center gap-1.5 text-white font-medium text-sm select-none">
                                {/* Ponemos el botón aquí sin texto para simular el contador interactivo de IG */}
                                <BotonLike 
                                    idPublicacion={postId} 
                                    idUsuarioActual={userId} 
                                    className="flex items-center gap-1 text-white bg-transparent border-none p-0 cursor-pointer pointer-events-auto"
                                    iconSize="16"
                                />
                            </div>
                        </div>
                    )}

                    {/* Titulo */}
                    <p className="font-semibold text-gray-800 truncate" title={tituloStr}>
                        {tituloStr}
                    </p>

                    {/* Descripcion */}
                    <p className="text-xs text-gray-400 mt-1 line-clamp-2" title={descripcionStr}>
                        {descripcionStr}
                    </p>
                </div>

                {/* BOTONES PRINCIPALES DE TARJETA */}
                <div className="flex items-center gap-3 mt-4">
                    {/* BOTONLIKE */}
                    <BotonLike 
                        idPublicacion={postId} 
                        idUsuarioActual={userId} 
                        iconSize="12"
                    />

                    {/* COMENTARIOS */}
                    <button
                        onClick={abrirDetalleModal}
                        className="flex items-center gap-1.5 px-3 py-1.5 rounded-full border border-gray-100 text-gray-400 hover:border-cyan-200 hover:text-cyan-600 transition text-xs font-medium cursor-pointer"
                    >
                        <svg width="12" height="12" viewBox="0 0 24 24"
                            fill="none" stroke="currentColor" strokeWidth="2">
                            <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />
                        </svg>
                        Comentarios
                    </button>
                </div>
            </div>

            {/* MODAL */}
            {showDetalleModal && (
                <div
                    className="fixed inset-0 z-50 flex items-center justify-center p-4"
                    style={{ backgroundColor: "rgba(0,0,0,0.65)" }}
                    onClick={() => setShowDetalleModal(false)}
                >
                    <div
                        className="bg-white rounded-2xl shadow-2xl w-full max-w-4xl h-[85vh] max-h-[750px] flex flex-col md:flex-row overflow-hidden relative"
                        onClick={(e) => e.stopPropagation()}
                    >
                        {/* Botón Cerrar Móvil */}
                        <button
                            onClick={() => setShowDetalleModal(false)}
                            className="absolute top-3 right-3 z-20 bg-black/40 hover:bg-black/60 text-white rounded-full w-8 h-8 flex items-center justify-center font-bold text-base cursor-pointer md:hidden"
                        >
                            ✕
                        </button>

                        <div className="md:w-3/5 bg-neutral-950 flex items-center justify-center relative select-none h-[40%] md:h-full">
                            {contenidoUrl ? (
                                <img
                                    src={contenidoUrl}
                                    alt={tituloStr}
                                    className="w-full h-full object-contain"
                                    onError={(e) =>
                                        (e.target.src = "https://placehold.co/600x400?text=Sin+imagen")
                                    }
                                />
                            ) : (
                                <div className="text-gray-400 text-sm">Sin vista previa</div>
                            )}
                        </div>

                        {/* SECCIÓN DERECHA MODAL */}
                        <div className="md:w-2/5 flex flex-col h-[60%] md:h-full bg-white relative">
                            {/* CABECERA */}
                            <div className="p-4 border-b border-gray-100 flex items-center justify-between">
                                <div className="flex items-center gap-3">
                                    {autorAvatar ? (
                                        <img
                                            src={autorAvatar}
                                            alt={autorNombre}
                                            className="w-8 h-8 rounded-full object-cover border border-cyan-100"
                                        />
                                    ) : (
                                        <div className="w-8 h-8 rounded-full bg-cyan-100 flex items-center justify-center text-sm font-semibold text-cyan-700">
                                            {autorNombre?.charAt(0).toUpperCase()}
                                        </div>
                                    )}
                                    <span className="font-semibold text-gray-800 text-sm">{autorNombre}</span>
                                </div>

                                <button
                                    onClick={() => setShowDetalleModal(false)}
                                    className="hidden md:flex text-gray-400 hover:text-gray-600 text-xl font-bold cursor-pointer"
                                >
                                    ✕
                                </button>
                            </div>

                            {/* CONTENIDO */}
                            <div className="flex-1 overflow-y-auto p-4 space-y-4">
                                <div className="flex items-start gap-3 pb-4 border-b border-gray-50">
                                    {autorAvatar ? (
                                        <img
                                            src={autorAvatar}
                                            alt={autorNombre}
                                            className="w-8 h-8 rounded-full object-cover"
                                        />
                                    ) : (
                                        <div className="w-8 h-8 rounded-full bg-cyan-100 flex items-center justify-center text-xs font-semibold text-cyan-700">
                                            {autorNombre?.charAt(0).toUpperCase()}
                                        </div>
                                    )}
                                    <div>
                                        <p className="text-sm font-semibold text-gray-800">
                                            {autorNombre} <span className="font-medium text-cyan-700 ml-1">@{tituloStr}</span>
                                        </p>
                                        <p className="text-xs text-gray-500 mt-1 whitespace-pre-wrap">{descripcionStr}</p>
                                    </div>
                                </div>

                                {/* Listado de Comentarios */}
                                {loadingCom ? (
                                    <p className="text-center text-gray-400 py-6 text-sm">Cargando comentarios...</p>
                                ) : errorCom ? (
                                    <p className="text-center text-red-400 py-6 text-sm">Error: {errorCom}</p>
                                ) : comentarios.length === 0 ? (
                                    <div className="flex flex-col items-center justify-center py-10 text-gray-300">
                                        <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.5">
                                            <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />
                                        </svg>
                                        <p className="text-xs mt-2 font-medium">Aún no hay comentarios</p>
                                        <p className="text-[10px] text-gray-400 mt-0.5">¡Sé el primero en comentar!</p>
                                    </div>
                                ) : (
                                    <div className="space-y-4">
                                        {comentarios.map((c) => (
                                            <div key={c.id} className="flex items-start gap-3">
                                                {c.usuarios?.avatar_url ? (
                                                    <img
                                                        src={c.usuarios.avatar_url}
                                                        alt={c.usuarios.nombre}
                                                        className="w-7 h-7 rounded-full object-cover"
                                                    />
                                                ) : (
                                                    <div className="w-7 h-7 rounded-full bg-cyan-100 flex items-center justify-center text-xs font-semibold text-cyan-700">
                                                        {c.usuarios?.nombre?.charAt(0).toUpperCase() ?? '?'}
                                                    </div>
                                                )}
                                                <div className="flex-1 min-w-0">
                                                    <p className="text-xs text-gray-800">
                                                        <span className="font-semibold">{c.usuarios?.nombre ?? "Usuario"}</span>
                                                        <span className="text-gray-600 ml-2 whitespace-pre-wrap">{c.contenido}</span>
                                                    </p>
                                                    <span className="text-[9px] text-gray-400 mt-1 block">
                                                        {c.fecha_comentario ? new Date(c.fecha_comentario).toLocaleDateString() : "Reciente"}
                                                    </span>
                                                </div>
                                            </div>
                                        ))}
                                    </div>
                                )}
                            </div>

                            {/* PIE DE PÁGINA*/}
                            <div className="p-4 border-t border-gray-100 bg-gray-50 flex flex-col gap-3">
                                <div className="flex items-center justify-between">
                                    <BotonLike 
                                        idPublicacion={postId} 
                                        idUsuarioActual={userId}
                                        iconSize="14"
                                        className="flex items-center gap-2 text-xs font-semibold text-gray-700 bg-transparent border-none p-0 cursor-pointer select-none"
                                    />
                                </div>

                                {/* Formulario para comentar */}
                                {usuarioLogueado ? (
                                    <form onSubmit={enviarComentario} className="flex gap-2">
                                        <input
                                            type="text"
                                            placeholder="Añade un comentario..."
                                            value={nuevoComentario}
                                            onChange={(e) => setNuevoComentario(e.target.value)}
                                            className="flex-1 bg-white border border-gray-200 rounded-lg px-3 py-2 text-xs focus:outline-none focus:ring-1 focus:ring-cyan-500 text-black placeholder-gray-400"
                                            disabled={submittingCom}
                                        />
                                        <button
                                            type="submit"
                                            className="bg-cyan-600 hover:bg-cyan-700 text-white font-medium text-xs px-3.5 py-2 rounded-lg transition-colors duration-200 disabled:opacity-50"
                                            disabled={submittingCom || !nuevoComentario.trim()}
                                        >
                                            {submittingCom ? "..." : "Publicar"}
                                        </button>
                                    </form>
                                ) : (
                                    <div className="text-center bg-gray-100 rounded-lg py-2 border border-gray-200">
                                        <p className="text-[10px] text-gray-500">
                                            Debes <span className="font-semibold text-cyan-600">iniciar sesión</span> para poder comentar.
                                        </p>
                                    </div>
                                )}
                            </div>
                        </div>
                    </div>
                </div>
            )}
        </>
    );
}


================================================
FILE: src/components/RegistroModal.jsx
================================================
import { useState } from "react";
import Swal from "sweetalert2";

function RegistroModal({ isOpen, onClose, alCambiarAModalLogin }) {
    const [isClosing, setIsClosing] = useState(false);
    const [formulario, setFormulario] = useState({
        nombre: "",
        apellido: "",
        telefono: "",
        correo: "",
        password: "",
        confirmarPassword: "",
    });

    if (!isOpen && !isClosing) return null;

    const manejarCierre = () => {
        setIsClosing(true);
        setTimeout(() => {
            onClose();
            setIsClosing(false);
        }, 300);
    };

    const manejarCambio = (e) => {
        setFormulario({
            ...formulario,
            [e.target.name]: e.target.value,
        });
    };

    const manejarRegistro = async (e) => {
        e.preventDefault();

        if (formulario.password !== formulario.confirmarPassword) {
            Swal.fire({
                icon: "error",
                title: "Error",
                text: "Las contraseñas no coinciden",
                confirmButtonColor: "#0891b2",
            });
            return;
        }

        try {
            const respuesta = await fetch(
                "http://localhost:3000/usuario/registro",
                {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                        nombre: formulario.nombre,
                        apellido: formulario.apellido,
                        telefono: formulario.telefono,
                        email: formulario.correo,
                        clave: formulario.password,
                        id_rol: 1,
                    }),
                },
            );

            const data = await respuesta.json();

            if (!respuesta.ok) {
                throw new Error(
                    data.error || "Error al registrar en el servidor",
                );
            }

            setFormulario({
                nombre: "",
                apellido: "",
                telefono: "",
                correo: "",
                password: "",
                confirmarPassword: "",
            });

            Swal.fire({
                icon: "success",
                title: "¡Registro exitoso!",
                text: "Ya puedes iniciar sesión en ArteNauta",
                confirmButtonColor: "#0891b2",
                timer: 1500,
                showConfirmButton: false,
            });

            onClose();
        } catch (error) {
            console.error("Error detallado:", error);
            Swal.fire({
                icon: "error",
                title: "Error al registrar",
                text: error.message || "No se pudo conectar con el servidor",
                confirmButtonColor: "#0891b2",
            });
        }
    };

    return (
        <div
            className="fixed inset-0 z-50 flex items-center justify-center backdrop-blur-sm animar-fondo"
            style={{ backgroundColor: "rgba(0,0,0,0.5)" }}
            onClick={onClose}
        >
            <div
                className={`bg-white rounded-2xl shadow-xl w-full max-w-md p-8 relative ${isClosing ? "animar-pop-salida" : "animar-pop"}`}
                onClick={(e) => e.stopPropagation()}
            >
                <button
                    onClick={manejarCierre}
                    className="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-2xl"
                >
                    ✕
                </button>
                <h2 className="text-2xl font-bold text-center text-cyan-600 mb-1">
                    Registro
                </h2>
                <p className="text-center text-gray-500 text-sm mb-6">
                    Empieza a formar parte de la comunidad artistica
                </p>
                <form
                    onSubmit={manejarRegistro}
                    className="flex flex-col gap-4"
                >
                    <div>
                        <label className="block text-sm font-medium text-gray-700 mb-1">
                            Nombre
                        </label>
                        <input
                            type="text"
                            name="nombre"
                            placeholder="Nombre de usuario"
                            className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500"
                            value={formulario.nombre}
                            onChange={manejarCambio}
                            required
                        />
                    </div>
                    <div>
                        <label className="block text-sm font-medium text-gray-700 mb-1">
                            Apellido
                        </label>
                        <input
                            type="text"
                            name="apellido"
                            placeholder="Ingresa tu apellido"
                            className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500"
                            value={formulario.apellido}
                            onChange={manejarCambio}
                            required
                        />
                    </div>
                    <div>
                        <label className="block text-sm font-medium text-gray-700 mb-1">
                            Telefono
                        </label>
                        <input
                            type="number"
                            name="telefono"
                            placeholder="Numero telefonico"
                            className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500 [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none"
                            value={formulario.telefono}
                            onChange={manejarCambio}
                            required
                        />
                    </div>
                    <div>
                        <label className="block text-sm font-medium text-gray-700 mb-1">
                            Email
                        </label>
                        <input
                            type="email"
                            name="correo"
                            placeholder="Correo Electronico"
                            className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500"
                            value={formulario.correo}
                            onChange={manejarCambio}
                            required
                        />
                    </div>
                    <div>
                        <label className="block text-sm font-medium text-gray-700 mb-1">
                            Contraseña
                        </label>
                        <input
                            type="password"
                            name="password"
                            placeholder="Contraseña"
                            className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500"
                            value={formulario.password}
                            onChange={manejarCambio}
                            required
                        />
                    </div>
                    <div>
                        <label className="block text-sm font-medium text-gray-700 mb-1">
                            Confirmar Contraseña
                        </label>
                        <input
                            type="password"
                            name="confirmarPassword"
                            placeholder="Vuelve a ingresar tu Contraseña"
                            className="text-black w-full border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-cyan-500"
                            value={formulario.confirmarPassword}
                            onChange={manejarCambio}
                            required
                        />
                    </div>
                    <button className="w-full bg-cyan-600 hover:bg-cyan-700 text-white font-semibold py-2 rounded-lg transition cursor-pointer">
                        Confirmar
                    </button>
                    <p className="text-center text-sm text-gray-500">
                        ¿Ya tienes una cuenta?{" "}
                        <span
                            onClick={alCambiarAModalLogin}
                            className="text-cyan-600 hover:underline cursor-pointer font-medium cursor pointer"
                        >
                            Inicia sesion
                        </span>
                    </p>
                </form>
            </div>
        </div>
    );
}

export default RegistroModal;



================================================
FILE: src/components/RestablecerClave.jsx
================================================
[Binary file]


================================================
FILE: src/components/paneles/AdminSidebar.jsx
================================================
function AdminSidebar({ seccion, setSeccion, abierto, onCerrar }) {
    const items = [
        {
            id: "inicio",
            label: "Inicio",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        strokeWidth={2}
                        d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"
                    />
                </svg>
            ),
        },
        {
            id: "VistaUsuario",
            label: "Ver como Usuario",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                    strokeWidth={2}
                    strokeLinecap="round"
                    strokeLinejoin="round"
                >
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z" />
                    <circle cx="12" cy="12" r="3" />
                </svg>
            ),
        },
        {
            id: "usuarios",
            label: "Usuarios",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        strokeWidth={2}
                        d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z"
                    />
                </svg>
            ),
        },
        {
            id: "publicaciones",
            label: "Publicaciones",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        strokeWidth={2}
                        d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
                    />
                </svg>
            ),
        },
        {
            id: "categorias",
            label: "Categorias",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                    strokeWidth={2}
                    strokeLinecap="round"
                    strokeLinejoin="round"
                >
                    <rect x="3" y="3" width="7" height="7" rx="1" />
                    <rect x="14" y="3" width="7" height="7" rx="1" />
                    <rect x="14" y="14" width="7" height="7" rx="1" />
                    <circle cx="6.5" cy="17.5" r="3.5" />
                </svg>
            ),
        },
        {
            id: "comentarios",
            label: "Comentarios",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        strokeWidth={2}
                        d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"
                    />
                </svg>
            ),
        },
        {
            id: "conversaciones",
            label: "Conversaciones",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        strokeWidth={2}
                        d="M17 8h2a2 2 0 012 2v6a2 2 0 01-2 2h-2v4l-4-4H9a1.994 1.994 0 01-1.414-.586m0 0L11 14h4a2 2 0 002-2V6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2v4l.586-.586z"
                    />
                </svg>
            ),
        },
        {
            id: "perfil",
            label: "Mi Perfil",
            icon: (
                <svg
                    className="w-5 h-5"
                    fill="none"
                    stroke="currentColor"
                    viewBox="0 0 24 24"
                >
                    <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        strokeWidth={2}
                        d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
                    />
                </svg>
            ),
        },
    ];

    const handleNavegar = (id) => {
        setSeccion(id);
        onCerrar();
    };

    return (
        <div>
            {abierto && (
                <div
                    className="fixed inset-0 z-30"
                    style={{ backgroundColor: "rgba(0, 0, 0, 0.25)" }}
                    onClick={onCerrar}
                />
            )}

            <aside
                className={`
                    fixed top-0 left-0 h-full w-64 z-40
                    bg-gradient-to-b from-cyan-900 to-cyan-800
                    text-white shadow-2xl
                    transform transition-transform duration-300 ease-in-out
                    ${abierto ? "translate-x-0" : "-translate-x-full"}
                `}
            >
                <div className="flex items-center justify-between p-4 border-b border-cyan-700">
                    <span className="font-bold text-lg tracking-wide">
                        ArteNauta
                    </span>
                    <button
                        onClick={onCerrar}
                        className="p-1 rounded hover:bg-cyan-700 transition"
                        aria-label="Cerrar menú"
                    >
                        <svg
                            className="w-5 h-5"
                            fill="none"
                            stroke="currentColor"
                            viewBox="0 0 24 24"
                        >
                            <path
                                strokeLinecap="round"
                                strokeLinejoin="round"
                                strokeWidth={2}
                                d="M6 18L18 6M6 6l12 12"
                            />
                        </svg>
                    </button>
                </div>

                <div className="px-4 py-3 bg-cyan-950 bg-opacity-40">
                    <span className="text-xs font-semibold uppercase tracking-widest text-cyan-300">
                        Administrador
                    </span>
                </div>

                <nav className="flex flex-col gap-1 p-3 mt-2">
                    {items.map((item) => (
                        <button
                            key={item.id}
                            onClick={() => handleNavegar(item.id)}
                            className={`
                                flex items-center gap-3 px-4 py-3 rounded-lg text-left
                                font-medium transition-all duration-150
                                ${
                                    seccion === item.id
                                        ? "bg-white text-cyan-800 shadow"
                                        : "hover:bg-cyan-700 hover:bg-opacity-60 text-white"
                                }
                            `}
                        >
                            {item.icon}
                            <span>{item.label}</span>

                            {seccion === item.id && (
                                <span className="ml-auto w-2 h-2 rounded-full bg-cyan-500" />
                            )}
                        </button>
                    ))}
                </nav>
            </aside>
        </div>
    );
}

export default AdminSidebar;



================================================
FILE: src/components/paneles/HeaderPanel.jsx
================================================
import Swal from "sweetalert2";
import Notificaciones from "./Notificaciones";
import AdminSidebar from "./AdminSidebar";

function HeaderPanel({
    seccion,
    setSeccion,
    setVista,
    onSubirArte,
    sidebarAbierto,
    setSidebarAbierto,
}) {
    const usuario = JSON.parse(localStorage.getItem("usuario"));
    const idRol = Number(usuario?.id_rol);

    const cerrarSesion = () => {
        Swal.fire({
            title: "¿Cerrar sesión?",
            icon: "question",
            showCancelButton: true,
            confirmButtonColor: "#0891b2",
            cancelButtonColor: "#d33",
            confirmButtonText: "Sí, salir",
            cancelButtonText: "Cancelar",
        }).then((result) => {
            if (result.isConfirmed) {
                localStorage.clear();
                setVista(null);
            }
        });
    };

    const btn = (seccionId) =>
        `px-4 py-2 rounded font-medium transition ${
            seccion === seccionId
                ? "bg-white text-cyan-700"
                : "bg-cyan-600 hover:bg-cyan-900"
        }`;

    const rolLabel = {
        3: "Panel Administrador",
        2: "Panel Artista",
        1: "Panel Usuario",
    };

    return (
        <div>
            <header className="bg-gradient-to-r from-cyan-900 to-cyan-400 text-white p-4 flex justify-between items-center shadow-lg">
                <div className="flex items-center gap-3">
                    {idRol === 3 && (
                        <button
                            onClick={() => setSidebarAbierto(!sidebarAbierto)}
                            className="p-2 rounded hover:bg-cyan-700 transition mr-1"
                            aria-label="Abrir menú"
                        >
                            <svg
                                className="w-6 h-6"
                                fill="none"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                            >
                                <path
                                    strokeLinecap="round"
                                    strokeLinejoin="round"
                                    strokeWidth={2}
                                    d="M4 6h16M4 12h16M4 18h16"
                                />
                            </svg>
                        </button>
                    )}

                    <img
                        src="../src/assets/LOGO.png"
                        alt="Logo"
                        className="h-20"
                        onError={(e) => (e.target.style.display = "none")}
                    />
                    <div className="flex flex-col">
                        <span className="font-bold text-lg leading-tight">
                            {rolLabel[idRol] || "ArteNauta"}
                        </span>
                        <span className="text-xs opacity-75">
                            Bienvenido, {usuario?.nombre}
                        </span>
                    </div>
                </div>

                <nav className="flex gap-2 items-center">
                    {idRol === 1 && (
                        <>
                            <button
                                onClick={() => setSeccion("inicio")}
                                className={btn("inicio")}
                            >
                                Inicio
                            </button>
                            <button
                                onClick={() => setSeccion("conversaciones")}
                                className={btn("conversaciones")}
                            >
                                Conversaciones
                            </button>
                            <button
                                onClick={() => setSeccion("perfil")}
                                className={btn("perfil")}
                            >
                                Mi Perfil
                            </button>
                        </>
                    )}

                    {idRol === 2 && (
                        <>
                            <button
                                onClick={() => setSeccion("inicio")}
                                className={btn("inicio")}
                            >
                                Inicio
                            </button>
                            <button
                                onClick={onSubirArte}
                                className="px-4 py-2 rounded font-medium transition bg-cyan-600 hover:bg-cyan-900"
                            >
                                Nueva Publicacion
                            </button>
                            <button
                                onClick={() => setSeccion("conversaciones")}
                                className={btn("conversaciones")}
                            >
                                Conversaciones
                            </button>
                            <button
                                onClick={() => setSeccion("perfil")}
                                className={btn("perfil")}
                            >
                                Mi Perfil
                            </button>
                        </>
                    )}

                    <Notificaciones usuario={usuario} setVista={setVista} />

                    <button
                        onClick={cerrarSesion}
                        className="px-4 py-2 rounded font-medium transition bg-cyan-600 hover:bg-red-800"
                    >
                        Cerrar Sesión
                    </button>
                </nav>
            </header>

            {idRol === 3 && (
                <AdminSidebar
                    seccion={seccion}
                    setSeccion={setSeccion}
                    abierto={sidebarAbierto}
                    onCerrar={() => setSidebarAbierto(false)}
                />
            )}
        </div>
    );
}

export default HeaderPanel;


================================================
FILE: src/components/paneles/MisObras.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";

const API_LOCAL_BACKEND = "http://localhost:3000";

// ✅ Recibe idUsuario y token como props desde PerfilUsuario
function MisObras({ idUsuario, token }) {
    const [misObras, setMisObras] = useState([]);
    const [loadingObras, setLoadingObras] = useState(false);
    const [obraEditando, setObraEditando] = useState(null);
    const [formObra, setFormObra] = useState({ titulo: "", descripcion: "" });

    // ✅ Headers reutilizables con el token en cada petición
    const headers = {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${token}`,
    };

    useEffect(() => {
        const obtenerMisObras = async () => {
            if (!idUsuario) return;
            try {
                setLoadingObras(true);
                const res = await fetch(
                    `${API_LOCAL_BACKEND}/mis-publicaciones/${idUsuario}`,
                    { headers }
                );
                if (!res.ok) throw new Error("Error al obtener tus publicaciones");
                const data = await res.json();
                setMisObras(data);
            } catch (err) {
                console.error(err);
            } finally {
                setLoadingObras(false);
            }
        };

        obtenerMisObras();
    }, [idUsuario]);

    const handleEliminarObra = async (idPublicacion) => {
        const confirm = await Swal.fire({
            title: "¿Eliminar esta obra?",
            text: "Esta acción no se puede deshacer.",
            icon: "warning",
            showCancelButton: true,
            confirmButtonColor: "#ef4444",
            cancelButtonColor: "#6b7280",
            confirmButtonText: "Sí, eliminar",
            cancelButtonText: "Cancelar",
        });

        if (!confirm.isConfirmed) return;

        try {
            const res = await fetch(
                `${API_LOCAL_BACKEND}/publicaciones/${idPublicacion}`,
                { method: "DELETE", headers }  // ✅ token incluido
            );
            if (!res.ok) throw new Error("No se pudo eliminar");

            setMisObras(misObras.filter(
                (o) => (o.id_publicacion || o.id) !== idPublicacion
            ));

            Swal.fire({ icon: "success", title: "Obra eliminada", timer: 1500, showConfirmButton: false });
        } catch (err) {
            Swal.fire("Error", "No se pudo eliminar la obra", "error");
        }
    };

    const abrirModarEditarObra = (obra) => {
        setObraEditando(obra);
        setFormObra({ titulo: obra.titulo, descripcion: obra.descripcion });
    };

    const handleGuardarCambiosObra = async () => {
        const idPublicacion = obraEditando.id_publicacion || obraEditando.id;
        try {
            const res = await fetch(
                `${API_LOCAL_BACKEND}/publicaciones/${idPublicacion}`,
                {
                    method: "PUT",
                    headers,                              // ✅ token incluido
                    body: JSON.stringify(formObra),
                }
            );
            if (!res.ok) throw new Error("Error al actualizar");

            setMisObras(misObras.map((o) =>
                (o.id_publicacion || o.id) === idPublicacion
                    ? { ...o, ...formObra }
                    : o
            ));
            setObraEditando(null);

            Swal.fire({ icon: "success", title: "Publicación actualizada", timer: 1500, showConfirmButton: false });
        } catch (err) {
            Swal.fire("Error", "No se pudieron guardar los cambios", "error");
        }
    };

    return (

        <div className="bg-white rounded-2xl shadow p-6 mb-6">
            <h3 className="text-lg font-bold text-cyan-800 mb-4">Mis Obras Publicadas</h3>
                
            {loadingObras && <p className="text-sm text-gray-400 text-center py-4">Cargando tus obras...</p>}

            {!loadingObras && misObras.length === 0 && (
                <p className="text-sm text-gray-400 text-center py-4">Aún no tienes obras compartidas en ArteNauta.</p>
            )}

            {!loadingObras && misObras.length > 0 && (
                <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    {misObras.map((obra) => (

                        <div key={obra.id_publicacion || obra.id} className="flex border border-gray-100 rounded-xl p-3 gap-3 bg-gray-50/50">
                            {obra.contenido && (
                                <img src={obra.contenido} alt={obra.titulo} className="w-20 h-20 object-cover rounded-lg bg-gray-200" />
                            )}
                            <div className="flex-1 min-w-0 flex flex-col justify-between">
                                <div>
                                    <h4 className="font-bold text-gray-800 text-sm truncate">{obra.titulo}</h4>
                                    <p className="text-xs text-gray-500 line-clamp-2 mt-0.5">{obra.descripcion}</p>
                                </div>
                                <div className="flex gap-2 mt-2">
                                    <button onClick={() => abrirModarEditarObra(obra)} className="text-xs bg-cyan-50 text-cyan-600 hover:bg-cyan-100 px-3 py-1.5 rounded-md font-medium transition">
                                        ✏️ Editar
                                    </button>
                                    <button onClick={() => handleEliminarObra(obra.id_publicacion || obra.id)} className="text-xs bg-red-50 text-red-600 hover:bg-red-100 px-3 py-1.5 rounded-md font-medium transition">
                                        🗑️ Eliminar
                                    </button>
                                </div>
                            </div>
                        </div>
                    ))}
                </div>
            )}

            {/* Modal editar obra */}
            {obraEditando && (
                <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm">
                    <div className="bg-white rounded-2xl p-6 w-full max-w-md mx-4 shadow-xl">
                        <h3 className="text-lg font-bold text-gray-800 mb-4">Editar Datos de la Obra</h3>
                        <div className="flex flex-col gap-4">
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">Título</label>
                                <input
                                    type="text"
                                    value={formObra.titulo}
                                    onChange={(e) => setFormObra({ ...formObra, titulo: e.target.value })}
                                    className="w-full p-2.5 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none"
                                />
                            </div>
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">Descripción</label>
                                <textarea
                                    rows="3"
                                    value={formObra.descripcion}
                                    onChange={(e) => setFormObra({ ...formObra, descripcion: e.target.value })}
                                    className="w-full p-2.5 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none resize-none"
                                />
                            </div>
                            <div className="flex gap-2 justify-end mt-2">
                                <button onClick={() => setObraEditando(null)} className="bg-gray-100 text-gray-600 text-sm font-medium px-4 py-2 rounded-xl hover:bg-gray-200 transition">
                                    Cancelar
                                </button>
                                <button onClick={handleGuardarCambiosObra} className="bg-cyan-600 text-white text-sm font-medium px-4 py-2 rounded-xl hover:bg-cyan-700 transition">
                                    Guardar Cambios
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            )}
        </div>
    );
}

export default MisObras;


================================================
FILE: src/components/paneles/Notificaciones.jsx
================================================
//Importaciones
import { useState, useEffect, useRef } from 'react';
import Swal from 'sweetalert2';
import { supabase } from '../../lib/supabase';
      
//Dirección del backend
const API = 'http://localhost:3000';

const iconosNotificacion = {
    solicitud_aprobada: (
        <svg className="w-5 h-5 text-green-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
    ),
    solicitud_rechazada: (
        <svg className="w-5 h-5 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
    ),
    censura_obra: (
        <svg className="w-5 h-5 text-red-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M18.364 18.364A9 9 0 005.636 5.636m12.728 12.728A9 9 0 015.636 5.636m12.728 12.728L5.636 5.636" />
        </svg>
    ),
    advertencia: (
        <svg className="w-5 h-5 text-yellow-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
        </svg>
    ),
    mensaje_admin: (
        <svg className="w-5 h-5 text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
        </svg>
    ),
    Mensaje: (
        <svg className="w-5 h-5 text-cyan-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
        </svg>
    ),
};

function Notificaciones({ usuario, setVista }) {
    const idUsuario = usuario?.id_usuario;
    const idRol = Number(usuario?.id_rol);
    const token = localStorage.getItem('token');

    const [notificaciones, setNotificaciones] = useState([]);
    const [solicitudes, setSolicitudes] = useState([]);
    const [abierto, setAbierto] = useState(false);
    const [vistas, setVistas] = useState(false); // ← nuevo
    const ref = useRef(null);

    const totalBadge = vistas ? 0 : notificaciones.length + solicitudes.length; // ← modificado

    const headers = {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`,
    };

    useEffect(() => {
        if (!idUsuario) return;
        cargar();

        const nombreCanalNotif = `notif-${idUsuario}`;
        const nombreCanalSol = `solicitudes-${idUsuario}`;
        const nombreCanalRol = `rol-usuario-${idUsuario}`;

        supabase.getChannels().forEach(ch => {
            const topic = ch.topic.replace('realtime:', '');
            if (topic === nombreCanalNotif || topic === nombreCanalSol || topic === nombreCanalRol) {
                supabase.removeChannel(ch);
            }
        });

        const canal = supabase
            .channel(nombreCanalNotif)
            .on('postgres_changes', {
                event: 'INSERT',
                schema: 'public',
                table: 'notificaciones',
                filter: `id_usuario=eq.${idUsuario}`,
            }, payload => {
                setNotificaciones(prev => [payload.new, ...prev]);
                setVistas(false); // ← badge vuelve al recibir nueva notif
            })
            .subscribe();

        let canalSol = null;
        if (idRol === 3) {
            canalSol = supabase
                .channel(nombreCanalSol)
                .on('postgres_changes', {
                    event: '*', schema: 'public', table: 'solicitudes',
                }, () => {
                    cargarSolicitudes();
                    setVistas(false); // ← badge vuelve al llegar solicitud
                })
                .subscribe();
        }

        const canalRol = supabase
            .channel(nombreCanalRol)
            .on('postgres_changes', {
                event: 'UPDATE',
                schema: 'public',
                table: 'usuarios',
                filter: `id_usuario=eq.${idUsuario}`,
            }, async (payload) => {
                const nuevoRol = Number(payload.new.id_rol);
                const usuarioActual = JSON.parse(localStorage.getItem('usuario'));
                if (nuevoRol !== Number(usuarioActual?.id_rol)) {
                    localStorage.setItem('usuario', JSON.stringify({ ...usuarioActual, id_rol: nuevoRol }));
                    await Swal.fire({
                        icon: 'success',
                        title: '🎉 ¡Ahora eres Artista!',
                        text: 'Serás redirigido a tu nuevo panel.',
                        confirmButtonColor: '#0891b2',
                        confirmButtonText: 'Ir a mi panel',
                    });
                    setVista(nuevoRol);
                }
            })
            .subscribe();

        return () => {
            supabase.removeChannel(canal);
            supabase.removeChannel(canalRol);
            if (canalSol) supabase.removeChannel(canalSol);
        };
    }, [idUsuario, idRol]);

    useEffect(() => {
        const fn = (e) => {
            if (ref.current && !ref.current.contains(e.target)) setAbierto(false);
        };
        document.addEventListener('mousedown', fn);
        return () => document.removeEventListener('mousedown', fn);
    }, []);

    async function cargar() {
        try {
            const res = await fetch(`${API}/notificaciones?id_usuario=${idUsuario}`, { headers });
            if (!res.ok) throw new Error('Error al cargar notificaciones');
            const data = await res.json();
            setNotificaciones(data);
        } catch (e) {
            console.error(e);
        }
        if (idRol === 3) cargarSolicitudes();
    }

    async function cargarSolicitudes() {
        try {
            const res = await fetch(`${API}/notificaciones/solicitudes`, { headers });
            if (!res.ok) throw new Error('Error al cargar solicitudes');
            const data = await res.json();
            setSolicitudes(data);
        } catch (e) {
            console.error(e);
        }
    }

    async function aprobarSolicitud(sol) {
        try {
            const res = await fetch(
                `${API}/notificaciones/solicitudes/${sol.id_solicitud}/aprobar`,
                { method: 'PATCH', headers, body: JSON.stringify({ id_usuario: sol.id_usuario }) }
            );
            if (!res.ok) throw new Error('Error al aprobar');
            setSolicitudes(prev => prev.filter(s => s.id_solicitud !== sol.id_solicitud));
            Swal.fire({ icon: 'success', title: '¡Solicitud aprobada!', confirmButtonColor: '#0891b2', timer: 2000, showConfirmButton: false });
        } catch (e) {
            console.error(e);
            Swal.fire({ icon: 'error', title: 'Error', text: e.message, confirmButtonColor: '#0891b2' });
        }
    }

    async function rechazarSolicitud(sol) {
        try {
            const res = await fetch(
                `${API}/notificaciones/solicitudes/${sol.id_solicitud}/rechazar`,
                { method: 'PATCH', headers, body: JSON.stringify({ id_usuario: sol.id_usuario }) }
            );
            if (!res.ok) throw new Error('Error al rechazar');
            setSolicitudes(prev => prev.filter(s => s.id_solicitud !== sol.id_solicitud));
            Swal.fire({ icon: 'info', title: 'Solicitud rechazada', confirmButtonColor: '#0891b2', timer: 2000, showConfirmButton: false });
        } catch (e) {
            console.error(e);
            Swal.fire({ icon: 'error', title: 'Error', text: e.message, confirmButtonColor: '#0891b2' });
        }
    }

    function tiempoRelativo(fecha) {
        const diff = Date.now() - new Date(fecha).getTime();
        const m = Math.floor(diff / 60000);
        const h = Math.floor(diff / 3600000);
        const d = Math.floor(diff / 86400000);
        if (m < 1) return 'Ahora';
        if (m < 60) return `Hace ${m} min`;
        if (h < 24) return `Hace ${h}h`;
        return `Hace ${d}d`;
    }

    return (
        <div className="relative" ref={ref}>
            <button
                onClick={() => {
                    const abriendo = !abierto;
                    setAbierto(abriendo);
                    if (abriendo) setVistas(true); // ← badge desaparece al abrir
                }}
                className="relative bg-cyan-600 hover:bg-cyan-900 p-2 rounded transition"
                title="Notificaciones"
            >
                <svg className="w-5 h-5 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2}
                        d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6 6 0 10-12 0v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
                </svg>
                {totalBadge > 0 && (
                    <span className="absolute -top-1 -right-1 bg-red-500 text-white text-xs font-bold w-5 h-5 rounded-full flex items-center justify-center animate-pulse">
                        {totalBadge > 9 ? '9+' : totalBadge}
                    </span>
                )}
            </button>

            {abierto && (
                <div className="absolute right-0 mt-2 w-80 bg-white rounded-xl shadow-2xl border border-gray-100 z-50 overflow-hidden">
                    <div className="flex items-center justify-between px-4 py-3 bg-cyan-50 border-b border-cyan-100">
                        <h3 className="font-semibold text-cyan-800 text-sm">Notificaciones</h3>
                    </div>

                    <div className="max-h-96 overflow-y-auto">
                        {idRol === 3 && solicitudes.length > 0 && (
                            <div className="divide-y divide-cyan-100">
                                {solicitudes.map(sol => (
                                    <div key={sol.id_solicitud} className="px-4 py-3 bg-cyan-50/60 hover:bg-cyan-50 transition">
                                        <div className="flex items-start gap-2 mb-2">
                                            <svg className="w-5 h-5 text-cyan-600 flex-shrink-0 mt-0.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                                            </svg>
                                            <div>
                                                <p className="text-sm font-semibold text-cyan-950">
                                                    {sol.usuarios?.nombre} {sol.usuarios?.apellido} quiere ser artista
                                                </p>
                                                <p className="text-xs text-cyan-600">{tiempoRelativo(sol.fecha_solicitud)}</p>
                                            </div>
                                        </div>
                                        <div className="flex gap-2">
                                            <button onClick={() => aprobarSolicitud(sol)}
                                                className="flex-1 bg-green-500 hover:bg-green-600 text-white text-xs py-1.5 rounded-lg font-semibold transition flex items-center justify-center gap-1">
                                                <svg className="w-3.5 h-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2.5} d="M5 13l4 4L19 7" />
                                                </svg>
                                                Aprobar
                                            </button>
                                            <button onClick={() => rechazarSolicitud(sol)}
                                                className="flex-1 bg-red-400 hover:bg-red-500 text-white text-xs py-1.5 rounded-lg font-semibold transition flex items-center justify-center gap-1">
                                                <svg className="w-3.5 h-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2.5} d="M6 18L18 6M6 6l12 12" />
                                                </svg>
                                                Rechazar
                                            </button>
                                        </div>
                                    </div>
                                ))}
                            </div>
                        )}

                        {notificaciones.length === 0 && solicitudes.length === 0 && (
                            <div className="p-8 text-center flex flex-col items-center justify-center">
                                <svg className="w-10 h-10 text-gray-300 mb-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6 6 0 10-12 0v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
                                </svg>
                                <p className="text-gray-400 text-sm">Sin notificaciones</p>
                            </div>
                        )}

                        {notificaciones.length > 0 && (
                            <div className="divide-y divide-gray-50">
                                {notificaciones.map(n => (
                                    <div key={n.id_notificacion} className="flex gap-3 px-4 py-3 hover:bg-gray-50 transition">
                                        <span className="flex-shrink-0 mt-0.5">
                                            {iconosNotificacion[n.tipo_notificacion] || (
                                                <svg className="w-5 h-5 text-cyan-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M11 5.882V19.24a1.76 1.76 0 01-3.417.592l-2.147-6.15M18 13a3 3 0 100-6M5.436 13.683A4.001 4.001 0 017 6h1.832c4.1 0 7.625-1.234 9.168-3v14c-1.543-1.766-5.067-3-9.168-3H7a3.988 3.988 0 01-1.564-.317z" />
                                                </svg>
                                            )}
                                        </span>
                                        <div className="flex-1 min-w-0">
                                            <p className="text-sm font-medium text-gray-800 leading-snug">{n.asunto}</p>
                                            <span className="text-xs text-gray-400 mt-1 block">{tiempoRelativo(n.fecha_notificacion)}</span>
                                        </div>
                                    </div>
                                ))}
                            </div>
                        )}
                    </div>
                </div>
            )}
        </div>
    );
}

export default Notificaciones;


================================================
FILE: src/components/paneles/PanelAdmin.jsx
================================================
import { useState } from "react";
import PerfilUsuario from "./PerfilUsuario";
import HeaderPanel from "./HeaderPanel";
import Conversaciones from "./ModulosConversaciones/VistaChats";
import Usuarios from "./ModulosAdmin/Usuarios";
import Estadisticas from "./ModulosAdmin/Estadisticas";
import Publicaciones from "./ModulosAdmin/Publicaciones";
import Comentarios from "./ModulosAdmin/Comentarios";
import Notificaciones from "./Notificaciones";
import VistaUsuario from "./ModulosAdmin/VistaUsuario";
import Categorias from "./ModulosAdmin/Categorias";

function PanelAdmin({ setVista }) {
    const usuario = JSON.parse(localStorage.getItem("usuario"));
    const [seccion, setSeccion] = useState("inicio");
    const [sidebarAbierto, setSidebarAbierto] = useState(false);

    return (
        <div className="min-h-screen bg-cyan-50 flex flex-col">
            <HeaderPanel
                seccion={seccion}
                setSeccion={setSeccion}
                setVista={setVista}
                sidebarAbierto={sidebarAbierto}
                setSidebarAbierto={setSidebarAbierto}
            />

            <main className="flex-1 p-8 max-w-5xl mx-auto w-full">
                {seccion === "inicio" && (
                    <div>
                        <h1 className="text-2xl font-bold text-cyan-800 mb-2">
                            Bienvenido, {usuario?.nombre}
                        </h1>
                        <p className="text-gray-500 mb-8">
                            Panel de administración de ArteNauta
                        </p>
                        <Estadisticas />
                    </div>
                )}

                {seccion === "VistaUsuario" && <VistaUsuario />}
                {seccion === "publicaciones" && <Publicaciones />}
                {seccion === "categorias" && <Categorias />}
                {seccion === "usuarios" && <Usuarios />}
                {seccion === "comentarios" && <Comentarios />}
                {seccion === "notificaciones" && (
                    <Notificaciones usuario={usuario} />
                )}
                {seccion === "conversaciones" && (
                    <Conversaciones usuario={usuario} />
                )}
                {seccion === "perfil" && <PerfilUsuario usuario={usuario} />}
            </main>
        </div>
    );
}

export default PanelAdmin;



================================================
FILE: src/components/paneles/PanelArtista.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";
import PerfilUsuario from "./PerfilUsuario";
import FormPublicaciones from "../FormPublicaciones";
import PublicacionesCom from "../PublicacionesCom";
import HeaderPanel from "./HeaderPanel";
import Conversaciones from "./ModulosConversaciones/VistaChats";
import Notificaciones from "./Notificaciones";

const API_LOCAL_BACKEND = "http://localhost:3000";

function PanelArtista({ setVista }) {
    const usuario = JSON.parse(localStorage.getItem("usuario"));
    const [seccion, setSeccion] = useState("inicio");
    const [showModal, setShowModal] = useState(false);
    const [obras, setObras] = useState([]);
    const [publicaciones, setPublicaciones] = useState([]);
    const [loading, setLoading] = useState(true);
    const [busqueda, setBusqueda] = useState("");
    const [filtroFecha, setFiltroFecha] = useState("desc");
    const [filtroLikes, setFiltroLikes] = useState("");
    const [showModalFiltros, setShowModalFiltros] = useState(false);
    const obtenerPublicacionesMuro = async () => {
        const token = localStorage.getItem("token");
        try {
            setLoading(true);
            let url = `${API_LOCAL_BACKEND}/Muro-Publicaciones?buscar=${encodeURIComponent(busqueda)}&ordenFecha=${filtroFecha}`;
            if (filtroLikes) url += `&ordenLikes=${filtroLikes}`;

            const res = await fetch(url, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    "Authorization": `Bearer ${token}`,
                },
            });

            if (res.status === 401) {
                localStorage.removeItem("token");
                localStorage.removeItem("usuario");
                Swal.fire({
                    icon: "warning",
                    title: "Sesión Expirada",
                    text: "Tu sesión ha terminado por seguridad. Vuelve a ingresar a ArteNauta.",
                    confirmButtonColor: "#0891b2",
                }).then(() => window.location.reload());
                return;
            }

            if (!res.ok) throw new Error("Error al obtener las publicaciones del muro");
            const data = await res.json();
            setPublicaciones(data);
        } catch (err) {
            console.error(err);
            Swal.fire("Error", "No se pudieron cargar las obras del muro", "error");
        } finally {
            setLoading(false);
        }
    };

    useEffect(() => {
        if (seccion === "inicio") {
            obtenerPublicacionesMuro();
        }
    }, [seccion, busqueda, filtroFecha, filtroLikes]);

    const handleNuevaPublicacion = (nueva) => {
        setObras((prev) => [nueva, ...prev]);
        setShowModal(false);
    };

    return (
        <div className="min-h-screen bg-cyan-50 flex flex-col">
            <HeaderPanel
                seccion={seccion}
                setSeccion={setSeccion}
                setVista={setVista}
                onSubirArte={() => setShowModal(true)}
            />

            <main className="flex-1 p-8 max-w-6xl mx-auto w-full">
                {seccion === "inicio" && (
                    <div>
                        {/* Header con bienvenida y buscador */}
                        <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                            <div>
                                <h1 className="text-2xl font-bold text-cyan-800 mb-1">
                                    Bienvenido, {usuario?.nombre}
                                </h1>
                                <p className="text-gray-500 text-sm">Tu espacio creativo en ArteNauta</p>
                            </div>

                            {/*barra de búsqueda */}
                            <div className="flex items-center gap-2 w-full md:w-96">
                                <input
                                    type="text"
                                    placeholder="Buscar por título o descripción..."
                                    value={busqueda}
                                    onChange={(e) => setBusqueda(e.target.value)}
                                    className="w-full px-4 py-2 text-sm border border-cyan-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-cyan-400 bg-white shadow-sm text-gray-700"
                                />
                                                                <button
                                    onClick={() => setShowModalFiltros(true)}
                                    className="px-3 py-2 bg-cyan-600 text-white rounded-xl hover:bg-cyan-700 transition shadow-sm flex items-center gap-1 font-medium text-sm whitespace-nowrap"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        width="16"
                                        height="16"
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        stroke="currentColor"
                                        strokeWidth="2"
                                        strokeLinecap="round"
                                        strokeLinejoin="round"
                                    >
                                        <path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1-1-1.74v-.5a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"></path>
                                        <circle cx="12" cy="12" r="3"></circle>
                                    </svg>
                                    Filtros
                                </button>
                            </div>
                        </div>

                        {/* Muro de publicaciones */}
                        <div className="bg-white rounded-2xl shadow p-6">
                            <h2 className="text-lg font-bold text-cyan-700 mb-4">Obras destacadas</h2>

                            {loading && (
                                <p className="text-center text-gray-400 py-10">Cargando publicaciones...</p>
                            )}

                            {!loading && publicaciones.length === 0 && (
                                <p className="text-center text-gray-400 py-10">
                                    No se encontraron obras que coincidan con los filtros aplicados.
                                </p>
                            )}

                            {!loading && publicaciones.length > 0 && (
                                <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                                    {publicaciones.map((item) => (
                                        <PublicacionesCom key={item.id_publicacion || item.id} item={item} />
                                    ))}
                                </div>
                            )}
                        </div>
                    </div>
                )}

                {seccion === "notificaciones" && <Notificaciones usuario={usuario} />}
                {seccion === "conversaciones" && <Conversaciones usuario={usuario} />}
                {seccion === "perfil" && <PerfilUsuario usuario={usuario} setSeccion={setSeccion} />}
            </main>

            {/* Modal subir obra */}
            {showModal && (
                <FormPublicaciones
                    onNuevaPublicacion={handleNuevaPublicacion}
                    onClose={() => setShowModal(false)}
                    idArtistaActivo={usuario?.id}
                />
            )}

            {showModalFiltros && (
                <div
                    className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm"
                    onClick={() => setShowModalFiltros(false)}
                >
                    <div
                        className="bg-white rounded-2xl p-6 w-full max-w-sm mx-4 shadow-xl"
                        onClick={(e) => e.stopPropagation()}
                    >
                        <div className="flex justify-between items-center mb-4">
                            <h3 className="text-lg font-bold text-gray-700">Filtros Avanzados</h3>
                            <button onClick={() => setShowModalFiltros(false)} className="text-gray-400 hover:text-gray-600 font-bold text-lg">✕</button>
                        </div>

                        <div className="flex flex-col gap-4">
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">Ordenar por Fecha:</label>
                                <select
                                    value={filtroFecha}
                                    onChange={(e) => { setFiltroFecha(e.target.value); setFiltroLikes(""); }}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="desc">Más recientes primero</option>
                                    <option value="asc">Más antiguas primero</option>
                                </select>
                            </div>
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">Ordenar por Popularidad (Likes):</label>
                                <select
                                    value={filtroLikes}
                                    onChange={(e) => setFiltroLikes(e.target.value)}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="">Por defecto</option>
                                    <option value="desc">Más populares</option>
                                    <option value="asc">Menos populares</option>
                                </select>
                            </div>
                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="mt-2 w-full bg-cyan-600 hover:bg-cyan-700 text-white font-medium py-2 rounded-xl text-sm transition"
                            >
                                Aplicar filtros
                            </button>
                        </div>
                    </div>
                </div>
            )}
        </div>
    );
}

export default PanelArtista;


================================================
FILE: src/components/paneles/PanelUsuario.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";

import PerfilUsuario from "./PerfilUsuario";
import PublicacionesCom from "../PublicacionesCom";
import HeaderPanel from "./HeaderPanel";
import Conversaciones from "./ModulosConversaciones/VistaChats";
import Notificaciones from "./ModulosConversaciones/../Notificaciones";

const API_LOCAL_BACKEND = "http://localhost:3000";

function PanelUsuario({ setVista }) {
    const usuario = JSON.parse(localStorage.getItem("usuario"));
    const [publicaciones, setPublicaciones] = useState([]);
    const [loading, setLoading] = useState(true);
    const [seccion, setSeccion] = useState("inicio");
    const [busqueda, setBusqueda] = useState("");
    const [filtroFecha, setFiltroFecha] = useState("desc"); 
    const [filtroLikes, setFiltroLikes] = useState(""); 
    const [showModalFiltros, setShowModalFiltros] = useState(false);

    const obtenerPublicacionesMuro = async () => {
        const token = localStorage.getItem("token");

        try {
            setLoading(true);

            let url = `${API_LOCAL_BACKEND}/Muro-Publicaciones?buscar=${encodeURIComponent(busqueda)}&ordenFecha=${filtroFecha}`;
            if (filtroLikes) {
                url += `&ordenLikes=${filtroLikes}`;
            }

            const res = await fetch(url, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
            });

            if (res.status === 401) {
                console.log("Sesión expirada o token inválido");

                localStorage.removeItem("token");
                localStorage.removeItem("usuario");

                Swal.fire({
                    icon: "warning",
                    title: "Sesión Expirada",
                    text: "Tu sesión ha terminado por seguridad. Vuelve a ingresar a ArteNauta.",
                    confirmButtonColor: "#0891b2",
                }).then(() => {
                    window.location.reload();
                });

                return;
            }

            if (!res.ok)
                throw new Error("Error al obtener las publicaciones del muro");

            const data = await res.json();
            setPublicaciones(data);
        } catch (err) {
            console.error(err);
            Swal.fire(
                "Error",
                "No se pudieron cargar las obras del muro",
                "error",
            );
        } finally {
            setLoading(false);
        }
    };
    useEffect(() => {
        if (seccion === "inicio") {
            obtenerPublicacionesMuro();
        }
    }, [seccion, busqueda, filtroFecha, filtroLikes]);

    return (
        <div className="min-h-screen bg-cyan-50 flex flex-col">
            <HeaderPanel
                seccion={seccion}
                setSeccion={setSeccion}
                setVista={setVista}
            />

            <main className="flex-1 p-4 max-w-7xl mx-auto w-full">
                {seccion === "inicio" && (
                    <div>
                        <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                            <div>
                                <h1 className="text-2xl font-bold text-cyan-800 mb-1">
                                    Bienvenido, {usuario?.nombre}
                                </h1>
                                <p className="text-gray-500 text-sm">
                                    Explora y descubre arte en ArteNauta
                                </p>
                            </div>

                            <div className="flex items-center gap-2 w-full md:w-96">
                                <input
                                    type="text"
                                    placeholder="Buscar por título o descripción..."
                                    value={busqueda}
                                    onChange={(e) =>
                                        setBusqueda(e.target.value)
                                    }
                                    className="w-full px-4 py-2 text-sm border border-cyan-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-cyan-400 bg-white shadow-sm text-gray-700"
                                />
                                <button
                                    onClick={() => setShowModalFiltros(true)}
                                    className="px-3 py-2 bg-cyan-600 text-white rounded-xl hover:bg-cyan-700 transition shadow-sm flex items-center gap-1 font-medium text-sm whitespace-nowrap"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        width="16"
                                        height="16"
                                        viewBox="0 0 24 24"
                                        fill="none"
                                        stroke="currentColor"
                                        strokeWidth="2"
                                        strokeLinecap="round"
                                        strokeLinejoin="round"
                                    >
                                        <path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1-1-1.74v-.5a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"></path>
                                        <circle cx="12" cy="12" r="3"></circle>
                                    </svg>
                                    Filtros
                                </button>
                            </div>
                        </div>

                        <div className="bg-white rounded-2xl shadow p-6">
                            <h2 className="text-lg font-bold text-cyan-700 mb-4">
                                Obras destacadas
                            </h2>

                            {loading && (
                                <p className="text-center text-gray-400 py-10">
                                    Cargando publicaciones...
                                </p>
                            )}

                            {!loading && publicaciones.length === 0 && (
                                <p className="text-center text-gray-400 py-10">
                                    No se encontraron obras que coincidan con
                                    los filtros aplicados.
                                </p>
                            )}

                            {!loading && publicaciones.length > 0 && (
                                <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                                    {publicaciones.map((item) => (
                                        <PublicacionesCom
                                            key={item.id_publicacion || item.id}
                                            item={item}
                                        />
                                    ))}
                                </div>
                            )}
                        </div>
                    </div>
                )}

                {seccion === "notificaciones" && (
                    <Notificaciones usuario={usuario} />
                )}

                {seccion === "conversaciones" && (
                    <Conversaciones usuario={usuario} />
                )}

                {seccion === "perfil" && (
                    <PerfilUsuario usuario={usuario} setSeccion={setSeccion} />
                )}
            </main>

            {showModalFiltros && (
                <div
                    className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm"
                    onClick={() => setShowModalFiltros(false)}
                >
                    <div
                        className="bg-white rounded-2xl p-6 w-full max-w-sm mx-4 shadow-xl"
                        onClick={(e) => e.stopPropagation()}
                    >
                        <div className="flex justify-between items-center mb-4">
                            <h3 className="text-lg font-bold text-gray-700">
                                Filtros Avanzados
                            </h3>
                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="text-gray-400 hover:text-gray-600 font-bold text-lg"
                            >
                                ✕
                            </button>
                        </div>

                        <div className="flex flex-col gap-4">
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">
                                    Ordenar por Fecha:
                                </label>
                                <select
                                    value={filtroFecha}
                                    onChange={(e) => {
                                        setFiltroFecha(e.target.value);
                                        setFiltroLikes(""); 
                                    }}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="desc">
                                        Más recientes primero
                                    </option>
                                    <option value="asc">
                                        Más antiguas primero
                                    </option>
                                </select>
                            </div>

                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">
                                    Ordenar por Popularidad (Likes):
                                </label>
                                <select
                                    value={filtroLikes}
                                    onChange={(e) =>
                                        setFiltroLikes(e.target.value)
                                    }
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="">
                                        Por defecto (Sin aplicar filtro de
                                        likes)
                                    </option>
                                    <option value="desc">
                                        Más populares (Mayor a menor)
                                    </option>
                                    <option value="asc">
                                        Menos populares (Menor a mayor)
                                    </option>
                                </select>
                            </div>

                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="mt-2 w-full bg-cyan-600 hover:bg-cyan-700 text-white font-medium py-2 rounded-xl text-sm transition"
                            >
                                Aplicar filtros
                            </button>
                        </div>
                    </div>
                </div>
            )}
        </div>
    );
}

export default PanelUsuario;



================================================
FILE: src/components/paneles/PerfilUsuario.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";
import { supabase } from "../../lib/supabase";
import MisObras from "./MisObras";

const API_LOCAL_BACKEND = "http://localhost:3000";

function PerfilUsuario({ usuario, setSeccion }) {
    const token = localStorage.getItem("token");

    const [perfil, setPerfil] = useState(null); 
    const [cargandoPerfil, setCargandoPerfil] = useState(true);

    const [editandoDatos, setEditandoDatos] = useState(false);
    const [guardandoDatos, setGuardandoDatos] = useState(false);
    const [formDatos, setFormDatos] = useState({ nombre: "", apellido: "", telefono: "" });

    const [editandoExtra, setEditandoExtra] = useState(false);
    const [guardandoExtra, setGuardandoExtra] = useState(false);
    const [formExtra, setFormExtra] = useState({ descripcion: "", ocupacion: "" });

    const [solicitando, setSolicitando] = useState(false);

    const rolMap = { 1: "Usuario Final", 2: "Artista", 3: "Admin" };
    const rolLabel = rolMap[usuario?.id_rol] || "usuario";
    const rolKey = { 1: "usuario", 2: "artista", 3: "admin" }[usuario?.id_rol] || "usuario";
    const rolColor = {
        admin: "bg-red-100 text-red-700 border border-red-300",
        artista: "bg-cyan-100 text-cyan-700 border border-cyan-300",
        usuario: "bg-green-100 text-green-700 border border-green-300",
    };

    const inicial = (perfil?.nombre || usuario?.nombre || "U")[0].toUpperCase();

    const obtenerPerfil = async () => {
        try {
            setCargandoPerfil(true);
            const res = await fetch(`${API_LOCAL_BACKEND}/perfil`, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
            });

            if (!res.ok) throw new Error("Error al obtener el perfil");

            const data = await res.json();
            setPerfil(data);
            setFormDatos({
                nombre: data.nombre || "",
                apellido: data.apellido || "",
                telefono: data.telefono || "",
            });
            setFormExtra({
                descripcion: data.descripcion || "",
                ocupacion: data.ocupacion || "",
            });
        } catch (err) {
            console.error(err);
            Swal.fire({
                icon: "error",
                title: "Ups...",
                text: "No se pudo cargar tu perfil",
                confirmButtonColor: "#0891b2",
                confirmButtonText: "Entendido",
            });
        } finally {
            setCargandoPerfil(false);
        }
    };

    useEffect(() => {
        obtenerPerfil();
    }, []);

    const handleGuardarDatos = async () => {
        setGuardandoDatos(true);
        try {
            const res = await fetch(`${API_LOCAL_BACKEND}/perfil/usuario`, {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
                body: JSON.stringify({
                    nombre: formDatos.nombre,
                    apellido: formDatos.apellido,
                    telefono: formDatos.telefono,
                }),
            });

            const data = await res.json();
            if (!res.ok) throw new Error(data.error || "Error al actualizar tus datos");

            setPerfil((prev) => ({ ...prev, ...formDatos }));
            setEditandoDatos(false);

            Swal.fire({
                icon: "success",
                title: "¡Perfil actualizado!",
                confirmButtonColor: "#0891b2",
                timer: 1800,
                showConfirmButton: false,
            });
        } catch (err) {
            console.error(err);
            Swal.fire({
                icon: "error",
                title: "Ups...",
                text: err.message || "No se pudieron guardar tus datos",
                confirmButtonColor: "#0891b2",
                confirmButtonText: "Intentar de nuevo",
            });
        } finally {
            setGuardandoDatos(false);
        }
    };

    const handleGuardarExtra = async () => {
        setGuardandoExtra(true);
        try {
            const res = await fetch(`${API_LOCAL_BACKEND}/perfil`, {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
                body: JSON.stringify({
                    descripcion: formExtra.descripcion,
                    ocupacion: formExtra.ocupacion,
                }),
            });

            const data = await res.json();
            if (!res.ok) throw new Error(data.error || "Error al guardar la información");

            setPerfil((prev) => ({
                ...prev,
                descripcion: data.descripcion,
                ocupacion: data.ocupacion,
                foto_perfil: data.foto_perfil,
            }));
            setEditandoExtra(false);

            Swal.fire({
                icon: "success",
                title: "¡Información guardada!",
                confirmButtonColor: "#0891b2",
                timer: 1800,
                showConfirmButton: false,
            });
        } catch (err) {
            console.error(err);
            Swal.fire({
                icon: "error",
                title: "Ups...",
                text: err.message || "No se pudo guardar la información",
                confirmButtonColor: "#0891b2",
                confirmButtonText: "Intentar de nuevo",
            });
        } finally {
            setGuardandoExtra(false);
        }
    };

    const handleSolicitarArtista = async () => {
        const confirm = await Swal.fire({
            title: "¿Solicitar ser artista?",
            text: "Se enviará una solicitud al administrador para cambiar tu rol.",
            icon: "question",
            showCancelButton: true,
            confirmButtonColor: "#0891b2",
            cancelButtonColor: "#d33",
            confirmButtonText: "Sí, solicitar",
            cancelButtonText: "Cancelar",
        });

        if (!confirm.isConfirmed) return;

        setSolicitando(true);
        try {
            const { data: existentes, error: errCheck } = await supabase
                .from("solicitudes")
                .select("id_solicitud")
                .eq("id_usuario", usuario?.id_usuario)
                .eq("estado_solicitud", "Pendiente");

            if (errCheck) throw errCheck;

            if (existentes && existentes.length > 0) {
                Swal.fire({
                    icon: "info",
                    title: "Ya tienes una solicitud pendiente",
                    text: "El administrador aún no ha respondido.",
                    confirmButtonColor: "#0891b2",
                });
                return;
            }

            const { error: errInsert } = await supabase
                .from("solicitudes")
                .insert({
                    id_usuario: usuario?.id_usuario,
                    fecha_solicitud: new Date().toISOString(),
                    tipo_solicitud: "artista",
                    estado_solicitud: "Pendiente",
                });

            if (errInsert) throw errInsert;

            Swal.fire({
                icon: "success",
                title: "¡Solicitud enviada!",
                text: "El administrador revisará tu solicitud pronto.",
                confirmButtonColor: "#0891b2",
                timer: 2500,
                showConfirmButton: false,
            });
        } catch (error) {
            Swal.fire({
                icon: "error",
                title: "Error",
                text: error?.message || "No se pudo enviar la solicitud.",
                confirmButtonColor: "#0891b2",
            });
        } finally {
            setSolicitando(false);
        }
    };

    if (cargandoPerfil) {
        return (
            <div className="max-w-4xl mx-auto py-20 text-center text-gray-400">
                Cargando perfil...
            </div>
        );
    }

    const tieneInfoExtra = perfil?.descripcion || perfil?.ocupacion || perfil?.foto_perfil;

    return (
        <div className="max-w-4xl mx-auto">
            <h1 className="text-2xl font-bold text-cyan-800 mb-6">Mi Perfil</h1>

            <div className="bg-white rounded-2xl shadow p-6 mb-6 flex flex-col sm:flex-row gap-6 items-start">
                <div className="flex flex-col items-center gap-2 min-w-[90px]">
                    {perfil?.foto_perfil ? (
                        <img
                            src={perfil.foto_perfil}
                            alt="Foto de perfil"
                            className="w-20 h-20 rounded-full object-cover shadow"
                        />
                    ) : (
                        <div className="w-20 h-20 rounded-full bg-gradient-to-br from-cyan-500 to-cyan-800 flex items-center justify-center text-white text-3xl font-bold shadow">
                            {inicial}
                        </div>
                    )}
                    <span className={`text-xs font-semibold px-3 py-1 rounded-full capitalize ${rolColor[rolKey]}`}>
                        {rolLabel}
                    </span>
                </div>

                <div className="flex-1 w-full">
                    {editandoDatos ? (
                        <div className="flex flex-col gap-3">
                            <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
                                <div>
                                    <label className="text-xs font-medium text-gray-500 mb-1 block">Nombre</label>
                                    <input
                                        className="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400"
                                        value={formDatos.nombre}
                                        onChange={(e) => setFormDatos({ ...formDatos, nombre: e.target.value })}
                                    />
                                </div>
                                <div>
                                    <label className="text-xs font-medium text-gray-500 mb-1 block">Apellido</label>
                                    <input
                                        className="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400"
                                        value={formDatos.apellido}
                                        onChange={(e) => setFormDatos({ ...formDatos, apellido: e.target.value })}
                                    />
                                </div>
                            </div>
                            <div>
                                <label className="text-xs font-medium text-gray-500 mb-1 block">Teléfono</label>
                                <input
                                    type="tel"
                                    className="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400"
                                    value={formDatos.telefono}
                                    onChange={(e) => setFormDatos({ ...formDatos, telefono: e.target.value })}
                                />
                            </div>
                            <div className="flex gap-2 mt-1">
                                <button
                                    onClick={handleGuardarDatos}
                                    disabled={guardandoDatos}
                                    className="bg-cyan-600 hover:bg-cyan-700 disabled:opacity-60 text-white px-5 py-2 rounded-lg text-sm font-semibold transition"
                                >
                                    {guardandoDatos ? "Guardando..." : "Guardar"}
                                </button>
                                <button
                                    onClick={() => setEditandoDatos(false)}
                                    disabled={guardandoDatos}
                                    className="bg-gray-100 hover:bg-gray-200 text-gray-600 px-5 py-2 rounded-lg text-sm font-semibold transition"
                                >
                                    Cancelar
                                </button>
                            </div>
                        </div>
                    ) : (
                        <div>
                            <h2 className="text-xl font-bold text-gray-800 mb-1">
                                {perfil?.nombre} {perfil?.apellido}
                            </h2>
                            <p className="text-gray-400 text-sm mb-3">
                                @{(perfil?.nombre || "usuario").toLowerCase()}
                            </p>
                            <div className="flex flex-col gap-1 text-sm text-gray-600 mb-4">
                                {perfil?.email && (
                                    <span><span className="font-medium">Email:</span> {perfil.email}</span>
                                )}
                                {perfil?.telefono && (
                                    <span><span className="font-medium">Teléfono:</span> {perfil.telefono}</span>
                                )}
                            </div>
                            <button
                                onClick={() => setEditandoDatos(true)}
                                className="border border-gray-300 hover:border-cyan-500 hover:text-cyan-600 text-gray-500 px-4 py-2 rounded-lg text-sm font-semibold transition"
                            >
                                Editar Perfil
                            </button>
                        </div>
                    )}
                </div>
            </div>

            <div className="bg-white rounded-2xl shadow p-6 mb-6">
                {editandoExtra ? (
                    <div className="flex flex-col gap-3">
                        <h3 className="font-bold text-gray-800 text-sm mb-1">Información adicional</h3>
                        <div>
                            <label className="text-xs font-medium text-gray-500 mb-1 block">Ocupación</label>
                            <input
                                className="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400"
                                placeholder="Ej. Ilustradora freelance"
                                value={formExtra.ocupacion}
                                onChange={(e) => setFormExtra({ ...formExtra, ocupacion: e.target.value })}
                            />
                        </div>
                        <div>
                            <label className="text-xs font-medium text-gray-500 mb-1 block">Descripción</label>
                            <textarea
                                className="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-400 resize-none h-24"
                                placeholder="Cuéntanos un poco sobre ti..."
                                value={formExtra.descripcion}
                                onChange={(e) => setFormExtra({ ...formExtra, descripcion: e.target.value })}
                            />
                        </div>
                        <div className="flex gap-2 mt-1">
                            <button
                                onClick={handleGuardarExtra}
                                disabled={guardandoExtra}
                                className="bg-cyan-600 hover:bg-cyan-700 disabled:opacity-60 text-white px-5 py-2 rounded-lg text-sm font-semibold transition"
                            >
                                {guardandoExtra ? "Guardando..." : "Guardar"}
                            </button>
                            <button
                                onClick={() => setEditandoExtra(false)}
                                disabled={guardandoExtra}
                                className="bg-gray-100 hover:bg-gray-200 text-gray-600 px-5 py-2 rounded-lg text-sm font-semibold transition"
                            >
                                Cancelar
                            </button>
                        </div>
                    </div>
                ) : tieneInfoExtra ? (
                    <div>
                        <h3 className="font-bold text-gray-800 text-sm mb-2">Información adicional</h3>
                        {perfil?.ocupacion && (
                            <p className="text-sm text-gray-600 mb-1">
                                <span className="font-medium">Ocupación:</span> {perfil.ocupacion}
                            </p>
                        )}
                        {perfil?.descripcion && (
                            <p className="text-sm text-gray-600 mb-3">{perfil.descripcion}</p>
                        )}
                        <button
                            onClick={() => setEditandoExtra(true)}
                            className="border border-gray-300 hover:border-cyan-500 hover:text-cyan-600 text-gray-500 px-4 py-2 rounded-lg text-sm font-semibold transition"
                        >
                            Editar información
                        </button>
                    </div>
                ) : (
                    <div className="text-center py-4">
                        <p className="text-sm text-gray-400 mb-3">
                            Aún no has añadido información a tu perfil.
                        </p>
                        <button
                            onClick={() => setEditandoExtra(true)}
                            className="border-2 border-dashed border-cyan-300 hover:border-cyan-500 hover:bg-cyan-50 text-cyan-600 px-5 py-2 rounded-xl font-semibold transition text-sm"
                        >
                            + Añade info a tu perfil
                        </button>
                    </div>
                )}
            </div>

            {usuario?.id_rol === 1 && (
                <div className="bg-white rounded-2xl shadow p-6 mb-6">
                    <div className="flex items-center gap-3 mb-3">
                        <span className="text-2xl">🎨</span>
                        <div>
                            <h3 className="font-bold text-gray-800 text-sm">¿Quieres ser Artista?</h3>
                            <p className="text-xs text-gray-400">Solicita el cambio de rol al administrador.</p>
                        </div>
                    </div>
                    <button
                        onClick={handleSolicitarArtista}
                        disabled={solicitando}
                        className="w-full border-2 border-dashed border-cyan-300 hover:border-cyan-500 hover:bg-cyan-50 text-cyan-600 py-3 rounded-xl font-semibold transition text-sm disabled:opacity-50"
                    >
                        {solicitando ? "Enviando solicitud..." : "Solicitar ser Artista"}
                    </button>
                </div>
            )}

            {usuario?.id_rol === 2 && (
                <MisObras
                    idUsuario={usuario?.id_usuario}
                    token={token}
                />
            )}

            {setSeccion && (
                <button onClick={() => setSeccion("inicio")} className="text-sm text-cyan-600 hover:underline mt-2 block">
                    &larr; Volver al inicio
                </button>
            )}
        </div>
    );
}

export default PerfilUsuario;



================================================
FILE: src/components/paneles/SolicitarArtista.jsx
================================================
import { useState, useEffect } from "react";
import { supabase } from "../../lib/supabase";
import Swal from "sweetalert2";

function SolicitarArtista() {
    const usuario = JSON.parse(localStorage.getItem("usuario"));
    const idUsuario = usuario?.id_usuario;
    const idRol = usuario?.id_rol;

    const [solicitud, setSolicitud] = useState(null);
    const [mensaje, setMensaje] = useState("");
    const [cargando, setCargando] = useState(true);
    const [enviando, setEnviando] = useState(false);
    const [mostrarForm, setMostrarForm] = useState(false);

    // Solo visible para rol usuario (id_rol === 1)
    if (idRol !== 1) return null;

    useEffect(() => { cargar(); }, []);

    async function cargar() {
        const { data } = await supabase
            .from("solicitudes")
            .select("*")
            .eq("id_usuario", idUsuario)
            .eq("tipo_solicitud", "artista")
            .order("fecha_solicitud", { ascending: false })
            .limit(1)
            .maybeSingle();
        setSolicitud(data);
        setCargando(false);
    }

   async function enviar() {
    setEnviando(true);
    try {
        const { data: pendienteExistente, error: errorCheck } = await supabase
            .from("solicitudes")
            .select("id_solicitud")
            .eq("id_usuario", idUsuario)
            .eq("tipo_solicitud", "artista")
            .eq("estado_solicitud", "pendiente")
            .maybeSingle();

        if (errorCheck) throw errorCheck;

        if (pendienteExistente) {
            await cargar(); 
            Swal.fire({
                icon: "info",
                title: "Ya tienes una solicitud pendiente",
                text: "Espera la respuesta del administrador antes de enviar otra.",
                confirmButtonColor: "#0891b2",
            });
            setEnviando(false);
            return;
        }

        const { error } = await supabase
            .from("solicitudes")
            .insert({
                id_usuario: idUsuario,
                tipo_solicitud: "artista",
                estado_solicitud: "pendiente",
                mensaje: mensaje.trim() || null,
                fecha_solicitud: new Date().toISOString(),
            });

        if (error) throw error;

        const { data: admins } = await supabase
            .from("usuarios")
            .select("id_usuario")
            .eq("id_rol", 3);

        if (admins?.length) {
            await supabase.from("notificaciones").insert(
                admins.map(a => ({
                    id_usuario: a.id_usuario,
                    asunto: "Nueva solicitud de artista",
                    descripcion: `${usuario.nombre} quiere convertirse en artista.`,
                    tipo_notificacion: "nueva_solicitud_artista",
                    leida: false,
                    fecha_notificacion: new Date().toISOString(),
                }))
            );
        }

        setMostrarForm(false);
        setMensaje("");
        await cargar();

        Swal.fire({
            icon: "success",
            title: "¡Solicitud enviada!",
            text: "Te notificaremos cuando el administrador responda.",
            confirmButtonColor: "#0891b2",
            timer: 2500,
            showConfirmButton: false,
        });
    } catch (err) {
        console.error(err);
        Swal.fire({
            icon: "error",
            title: "Error",
            text: "No se pudo enviar la solicitud. Intenta de nuevo.",
            confirmButtonColor: "#0891b2",
        });
    } finally {
        setEnviando(false);
    }
}

    if (cargando) return null;

    // Ya tiene solicitud pendiente (Icono de reloj SVG)
    if (solicitud?.estado_solicitud === "pendiente") {
        return (
            <div className="flex gap-3 items-start p-4 bg-yellow-50 border border-yellow-200 rounded-xl mt-4">
                <svg className="w-6 h-6 text-yellow-600 flex-shrink-0 mt-0.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <div>
                    <p className="font-semibold text-yellow-800">Solicitud en revisión</p>
                    <p className="text-sm text-yellow-700 mt-1">
                        Tu solicitud está siendo revisada. Te notificaremos cuando haya una respuesta.
                    </p>
                </div>
            </div>
        );
    }

    // Rechazada — puede volver a intentar (Icono de advertencia SVG)
    if (solicitud?.estado_solicitud === "rechazada") {
        return (
            <div className="flex gap-3 items-start p-4 bg-red-50 border border-red-200 rounded-xl mt-4">
                <svg className="w-6 h-6 text-red-500 flex-shrink-0 mt-0.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
                </svg>
                <div>
                    <p className="font-semibold text-red-800">Solicitud no aprobada</p>
                    <p className="text-sm text-red-700 mt-1">Puedes volver a intentarlo.</p>
                    <button
                        onClick={() => setMostrarForm(true)}
                        className="mt-2 text-sm bg-cyan-600 hover:bg-cyan-700 text-white px-3 py-1 rounded-lg transition"
                    >
                        Volver a solicitar
                    </button>
                </div>
            </div>
        );
    }

    if (!mostrarForm) {
        return (
            <button
                onClick={() => setMostrarForm(true)}
                className="mt-4 flex items-center gap-2 px-4 py-2 bg-cyan-600 hover:bg-cyan-700 text-white font-semibold rounded-xl transition"
            >
                 Solicitar ser artista
            </button>
        );
    }

    return (
        <div className="mt-4 p-4 bg-gray-50 border border-gray-200 rounded-xl space-y-3">
            <h3 className="font-semibold text-gray-800">Solicitar ser artista</h3>
            <p className="text-sm text-gray-600">Cuéntanos sobre tu trabajo (opcional):</p>
            <textarea
                value={mensaje}
                onChange={e => setMensaje(e.target.value)}
                placeholder="Describe tu experiencia o motivación..."
                rows={3}
                maxLength={500}
                className="w-full border border-gray-300 rounded-lg px-3 py-2 text-sm resize-none focus:outline-none focus:ring-2 focus:ring-cyan-400"
            />
            <div className="flex gap-2 justify-end">
                <button
                    onClick={() => { setMostrarForm(false); setMensaje(""); }}
                    disabled={enviando}
                    className="px-4 py-2 text-sm border border-gray-300 rounded-lg hover:bg-gray-100"
                >
                    Cancelar
                </button>
                <button
                    onClick={enviar}
                    disabled={enviando}
                    className="px-4 py-2 text-sm bg-cyan-600 hover:bg-cyan-700 disabled:opacity-60 text-white font-semibold rounded-lg transition"
                >
                    {enviando ? "Enviando..." : "Enviar solicitud"}
                </button>
            </div>
        </div>
    );
}

export default SolicitarArtista;


================================================
FILE: src/components/paneles/ModulosAdmin/Categorias.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";

const API_LOCAL_BACKEND = "http://localhost:3000";

function Categorias() {
    const [categorias, setCategorias] = useState([]);
    const [loading, setLoading] = useState(true);
    const [guardando, setGuardando] = useState(false);
    const [busqueda, setBusqueda] = useState("");
    const [modalAbierto, setModalAbierto] = useState(false);
    const [modoEdicion, setModoEdicion] = useState(false);
    const [formulario, setFormulario] = useState({
        id_categoria: null,
        nombre_categoria: "",
        descripcion: ""
    });

    const obtenerCategorias = async (buscar = "") => {
        const token = localStorage.getItem("token");
        try {
            setLoading(true);
            const params = buscar ? `?buscar=${encodeURIComponent(buscar)}` : "";
            const res = await fetch(`${API_LOCAL_BACKEND}/categorias${params}`, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
            });

            if (!res.ok) throw new Error("Error al obtener las categorías");

            const data = await res.json();
            setCategorias(data);
        } catch (err) {
            console.error(err);
            Swal.fire({
                icon: "error",
                title: "Ups...",
                text: "No se pudieron cargar las categorías",
                confirmButtonColor: "#0891b2",
                confirmButtonText: "Entendido",
            });
        } finally {
            setLoading(false);
        }
    };

    useEffect(() => {
        obtenerCategorias();
    }, []);

    useEffect(() => {
        const timeoutId = setTimeout(() => {
            obtenerCategorias(busqueda);
        }, 400);

        return () => clearTimeout(timeoutId);
    }, [busqueda]);

    const abrirModalCrear = () => {
        setFormulario({ id_categoria: null, nombre_categoria: "", descripcion: "" });
        setModoEdicion(false);
        setModalAbierto(true);
    };

    const abrirModalEditar = (categoria) => {
        setFormulario({
            id_categoria: categoria.id_categoria,
            nombre_categoria: categoria.nombre_categoria || "",
            descripcion: categoria.descripcion || "",
        });
        setModoEdicion(true);
        setModalAbierto(true);
    };

    const guardarCategoria = async (e) => {
        e.preventDefault();
        const token = localStorage.getItem("token");
        setGuardando(true);

        try {
            if (modoEdicion) {
                const res = await fetch(`${API_LOCAL_BACKEND}/categorias/${formulario.id_categoria}`, {
                    method: "PATCH",
                    headers: {
                        "Content-Type": "application/json",
                        Authorization: `Bearer ${token}`,
                    },
                    body: JSON.stringify({
                        nombre_categoria: formulario.nombre_categoria,
                        descripcion: formulario.descripcion,
                    }),
                });

                const data = await res.json();
                if (!res.ok) throw new Error(data.error || "Error al editar la categoría");

                Swal.fire({
                    icon: "success",
                    title: "¡Listo!",
                    text: data.mensaje || "Categoría editada con éxito",
                    confirmButtonColor: "#0891b2",
                    confirmButtonText: "Aceptar",
                });
            } else {
                const res = await fetch(`${API_LOCAL_BACKEND}/categorias`, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                        Authorization: `Bearer ${token}`,
                    },
                    body: JSON.stringify({
                        nombre_categoria: formulario.nombre_categoria,
                        descripcion: formulario.descripcion,
                    }),
                });

                const data = await res.json();
                if (!res.ok) throw new Error(data.error || "Error al crear la categoría");

                Swal.fire({
                    icon: "success",
                    title: "¡Listo!",
                    text: "Categoría creada con éxito",
                    confirmButtonColor: "#0891b2",
                    confirmButtonText: "Aceptar",
                });
            }

            setModalAbierto(false);
            await obtenerCategorias(busqueda);
        } catch (err) {
            console.error(err);
            Swal.fire({
                icon: "error",
                title: "Ups...",
                text: err.message || "Ocurrió un error al guardar la categoría",
                confirmButtonColor: "#0891b2",
                confirmButtonText: "Intentar de nuevo",
            });
        } finally {
            setGuardando(false);
        }
    };

    return (
        <div className="w-full flex flex-col p-6">
            <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                <div>
                    <h1 className="text-2xl font-bold text-cyan-800 mb-1">
                        Gestión de Categorías
                    </h1>
                    <p className="text-gray-500 text-sm">
                        Administra las clasificaciones de las obras de ArteNauta.
                    </p>
                </div>

                <div className="flex items-center gap-4">
                    <input
                        type="text"
                        placeholder="Buscar categoría..."
                        value={busqueda}
                        onChange={(e) => setBusqueda(e.target.value)}
                        className="w-full md:w-64 px-4 py-2 text-sm border border-cyan-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-cyan-400 bg-white shadow-sm text-gray-700"
                    />
                    <button
                        onClick={abrirModalCrear}
                        className="px-4 py-2 bg-cyan-600 text-white rounded-xl hover:bg-cyan-700 transition shadow-sm font-medium text-sm whitespace-nowrap"
                    >
                        + Añadir Categoría
                    </button>
                </div>
            </div>

            <div className="bg-white rounded-2xl shadow border border-gray-100 overflow-hidden">
                {loading ? (
                    <p className="text-center text-gray-400 py-10">Cargando categorías...</p>
                ) : (
                    <table className="w-full text-left border-collapse">
                        <thead>
                            <tr className="bg-gray-50 border-b border-gray-100 text-gray-500 text-sm">
                                <th className="p-4 font-semibold">Nombre</th>
                                <th className="p-4 font-semibold">Descripción</th>
                                <th className="p-4 font-semibold text-center">Acciones</th>
                            </tr>
                        </thead>
                        <tbody>
                            {categorias.length > 0 ? (
                                categorias.map((cat) => (
                                    <tr key={cat.id_categoria} className="border-b border-gray-50 hover:bg-gray-50/50 transition">
                                        <td className="p-4 font-medium text-gray-700">{cat.nombre_categoria}</td>
                                        <td className="p-4 text-sm text-gray-500">{cat.descripcion}</td>
                                        <td className="p-4 text-center">
                                            <button
                                                onClick={() => abrirModalEditar(cat)}
                                                className="text-cyan-600 hover:text-cyan-800 text-sm font-medium mr-3"
                                            >
                                                Editar
                                            </button>
                                        </td>
                                    </tr>
                                ))
                            ) : (
                                <tr>
                                    <td colSpan="3" className="text-center text-gray-400 py-10">
                                        No se encontraron categorías.
                                    </td>
                                </tr>
                            )}
                        </tbody>
                    </table>
                )}
            </div>

            {modalAbierto && (
                <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm" onClick={() => !guardando && setModalAbierto(false)}>
                    <div className="bg-white rounded-2xl p-6 w-full max-w-md mx-4 shadow-xl" onClick={(e) => e.stopPropagation()}>
                        <div className="flex justify-between items-center mb-4">
                            <h3 className="text-lg font-bold text-gray-700">
                                {modoEdicion ? "Editar Categoría" : "Nueva Categoría"}
                            </h3>
                            <button onClick={() => setModalAbierto(false)} className="text-gray-400 hover:text-gray-600 font-bold text-lg">✕</button>
                        </div>

                        <form onSubmit={guardarCategoria} className="flex flex-col gap-4">
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">Nombre</label>
                                <input
                                    type="text"
                                    required
                                    value={formulario.nombre_categoria}
                                    onChange={(e) => setFormulario({...formulario, nombre_categoria: e.target.value})}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none"
                                />
                            </div>

                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">Descripción</label>
                                <textarea
                                    required
                                    value={formulario.descripcion}
                                    onChange={(e) => setFormulario({...formulario, descripcion: e.target.value})}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none resize-none h-20"
                                />
                            </div>

                            <button
                                type="submit"
                                disabled={guardando}
                                className="mt-2 w-full bg-cyan-600 hover:bg-cyan-700 disabled:opacity-60 text-white font-medium py-2 rounded-xl text-sm transition"
                            >
                                {guardando ? "Guardando..." : "Guardar Categoría"}
                            </button>
                        </form>
                    </div>
                </div>
            )}
        </div>
    );
}

export default Categorias;



================================================
FILE: src/components/paneles/ModulosAdmin/Comentarios.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";

const API_BACKEND = "http://localhost:3000";

function Comentarios() {
    const [comentarios, setComentarios] = useState([]);
    const [loading, setLoading] = useState(true);
    const [busqueda, setBusqueda] = useState("");

    const obtenerComentarios = async () => {
        const token = localStorage.getItem("token");
        try {
            setLoading(true);
            const res = await fetch(`${API_BACKEND}/admin/comentarios`, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
            });

            if (!res.ok) throw new Error("Error al obtener la lista de comentarios");

            const data = await res.json();
            setComentarios(data);
        } catch (err) {
            console.error(err);
            Swal.fire("Error", "No se pudieron cargar los comentarios", "error");
        } finally {
            setLoading(false);
        }
    };

    useEffect(() => {
        obtenerComentarios();
    }, []);
    const handleEliminar = async (idComentario) => {
        const confirmacion = await Swal.fire({
            title: "¿Eliminar comentario?",
            text: "Esta acción removerá el comentario de la obra de forma permanente",
            icon: "warning",
            showCancelButton: true,
            confirmButtonColor: "#d33",
            cancelButtonColor: "#0891b2",
            confirmButtonText: "Sí, eliminar",
            cancelButtonText: "Cancelar",
        });

        if (!confirmacion.isConfirmed) return;

        try {
            const token = localStorage.getItem("token");
            const res = await fetch(`${API_BACKEND}/comentarios/${idComentario}`, {
                method: "DELETE",
                headers: {
                    Authorization: `Bearer ${token}`,
                },
            });

            if (!res.ok) throw new Error("No se pudo eliminar el comentario");
            setComentarios(comentarios.filter((com) => (com.id_comentario || com.id) !== idComentario));

            Swal.fire({
                title: "Eliminado",
                text: "El comentario ha sido removido con éxito",
                icon: "success",
                timer: 1500,
                showConfirmButton: false,
            });
        } catch (err) {
            Swal.fire("Error", err.message, "error");
        }
    };
    const comentariosFiltrados = comentarios.filter((com) =>
        com.texto_comentario?.toLowerCase().includes(busqueda.toLowerCase()) ||
        com.usuarios?.nombre?.toLowerCase().includes(busqueda.toLowerCase()) ||
        com.publicaciones?.titulo?.toLowerCase().includes(busqueda.toLowerCase())
    );

    return (
        <div className="w-full flex flex-col p-6">
            {/* CABECERA DE LA VISTA */}
            <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                <div>
                    <h1 className="text-2xl font-bold text-cyan-800 mb-1">
                        Moderación de Comentarios
                    </h1>
                    <p className="text-gray-500 text-sm">
                        Revisa, filtra y elimina comentarios en las publicaciones de ArteNauta.
                    </p>
                </div>

                <div>
                    <input
                        type="text"
                        placeholder="Buscar por usuario, texto o publicación..."
                        value={busqueda}
                        onChange={(e) => setBusqueda(e.target.value)}
                        className="w-full md:w-80 px-4 py-2 text-sm border border-cyan-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-cyan-400 bg-white shadow-sm text-gray-700"
                    />
                </div>
            </div>

            {/* TABLA DE COMENTARIOS */}
            <div className="bg-white rounded-2xl shadow border border-gray-100 overflow-hidden">
                {loading ? (
                    <p className="text-center text-gray-400 py-10">Cargando comentarios...</p>
                ) : (
                    <div className="overflow-x-auto">
                        <table className="w-full text-left border-collapse">
                            <thead>
                                <tr className="bg-gray-50 border-b border-gray-100 text-gray-500 text-sm">
                                    <th className="p-4 font-semibold">Usuario</th>
                                    <th className="p-4 font-semibold">Comentario</th>
                                    <th className="p-4 font-semibold">Publicación (Obra)</th>
                                    <th className="p-4 font-semibold">Fecha</th>
                                    <th className="p-4 font-semibold text-center">Acciones</th>
                                </tr>
                            </thead>
                            <tbody>
                                {comentariosFiltrados.length > 0 ? (
                                    comentariosFiltrados.map((com) => {
                                        const currentId = com.id_comentario || com.id;
                                        return (
                                            <tr key={currentId} className="border-b border-gray-50 hover:bg-gray-50/50 transition text-sm">
                                                <td className="p-4">
                                                    <div className="font-medium text-gray-700">{com.usuarios?.nombre || "Usuario Desconocido"}</div>
                                                    <div className="text-xs text-gray-400">{com.usuarios?.email}</div>
                                                </td>
                                                <td className="p-4 text-gray-600 max-w-xs break-words italic">
                                                    "{com.texto_comentario || com.contenido}"
                                                </td>
                                                <td className="p-4 text-cyan-700 font-medium">
                                                    {com.publicaciones?.titulo || "Obra eliminada"}
                                                </td>
                                                <td className="p-4 text-gray-400 text-xs">
                                                    {com.fecha_comentario 
                                                        ? new Date(com.fecha_comentario).toLocaleDateString() 
                                                        : "Sin fecha"}
                                                </td>
                                                <td className="p-4 text-center">
                                                    <button 
                                                        onClick={() => handleEliminar(currentId)}
                                                        className="text-red-500 hover:text-red-700 hover:bg-red-50 px-3 py-1.5 rounded-xl transition text-xs font-semibold"
                                                    >
                                                        Eliminar
                                                    </button>
                                                </td>
                                            </tr>
                                        );
                                    })
                                ) : (
                                    <tr>
                                        <td colSpan="5" className="text-center text-gray-400 py-10">
                                            No se encontraron comentarios.
                                        </td>
                                    </tr>
                                )}
                            </tbody>
                        </table>
                    </div>
                )}
            </div>
        </div>
    );
}

export default Comentarios;


================================================
FILE: src/components/paneles/ModulosAdmin/Estadisticas.jsx
================================================
import { useState, useEffect } from 'react';
import Swal from 'sweetalert2';

const API_BASE = 'http://localhost:3000';

const StatCard = ({ label, value, colorBorder, textColor, icon }) => (
    <div className={`bg-white rounded-2xl shadow-sm p-6 border-l-4 ${colorBorder} flex items-center justify-between transition-all hover:shadow-md`}>
        <div>
            <p className="text-gray-400 font-medium text-xs uppercase tracking-wider">{label}</p>
            <p className={`text-3xl font-bold ${textColor} mt-1`}>{value}</p>
        </div>
        <div className={`p-3 rounded-xl bg-gray-50 text-gray-500`}>
            {icon}
        </div>
    </div>
);

function Estadisticas() {
    const [stats, setStats] = useState({
        totalUsuarios: 0,
        totalArtistas: 0,
        totalPublicaciones: 0,
        totalComentarios: 0,
    });
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        const obtenerEstadisticas = async () => {
            const token = localStorage.getItem("token");
            try {
                setLoading(true);
                const res = await fetch(`${API_BASE}/dashboard/estadisticas`, {
                    method: "GET",
                    headers: {
                        "Content-Type": "application/json",
                        "Authorization": `Bearer ${token}`
                    }
                });

                if (res.status === 401) return window.location.reload();
                if (!res.ok) throw new Error("No se pudieron cargar las métricas.");

                const data = await res.json();
                setStats(data);
            } catch (error) {
                console.error('Error cargando estadísticas:', error);
                Swal.fire("Error", "No se pudieron sincronizar los contadores en tiempo real", "error");
            } finally {
                setLoading(false);
            }
        };

        obtenerEstadisticas();
    }, []);

    if (loading) {
        return (
            <div className="flex items-center justify-center py-20">
                <p className="text-gray-400 text-sm animate-pulse">Sincronizando métricas en tiempo real...</p>
            </div>
        );
    }

    return (
        <div className="space-y-8">
            <div className="grid grid-cols-1 sm:grid-cols-1 lg:grid-cols-2 gap-6">
                
                {/* 1. Usuarios Registrados */}
                <StatCard
                    label="Usuarios en total"
                    value={stats.totalUsuarios}
                    colorBorder="border-cyan-500"
                    textColor="text-cyan-700"
                    icon={
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                            <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path>
                            <circle cx="9" cy="7" r="4"></circle>
                            <path d="M23 21v-2a4 4 0 0 0-3-3.87"></path>
                            <path d="M16 3.13a4 4 0 0 1 0 7.75"></path>
                        </svg>
                    }
                />

                <StatCard
                    label="Artistas verificados"
                    value={stats.totalArtistas}
                    colorBorder="border-purple-500"
                    textColor="text-purple-700"
                    icon={
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                            <path d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 14.7255 3.09032 17.1962 4.85857 19H12V22Z" />
                            <circle cx="12" cy="11" r="3" />
                        </svg>
                    }
                />

                <StatCard
                    label="Obras publicadas"
                    value={stats.totalPublicaciones}
                    colorBorder="border-green-500"
                    textColor="text-green-600"
                    icon={
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                            <rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect>
                            <circle cx="8.5" cy="8.5" r="1.5"></circle>
                            <polyline points="21 15 16 10 5 21"></polyline>
                        </svg>
                    }
                />
                
                <StatCard
                    label="Comentarios"
                    value={stats.totalComentarios}
                    colorBorder="border-amber-500"
                    textColor="text-amber-600"
                    icon={
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
                            <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path>
                        </svg>
                    }
                />
            </div>
        </div>
    );
}

export default Estadisticas;


================================================
FILE: src/components/paneles/ModulosAdmin/Publicaciones.jsx
================================================
import { useState, useEffect } from 'react';
import Swal from 'sweetalert2';

const API_PUBLICACIONES = 'http://localhost:3000/Muro-Publicaciones';

function Publicaciones() {
    const [publicaciones, setPublicaciones] = useState([]);
    const [busqueda, setBusqueda] = useState('');
    const [filtro, setFiltro] = useState('recientes');

    const obtenerPublicaciones = async () => {
        try {
            const token = localStorage.getItem('token');

            let url = `${API_PUBLICACIONES}?`;

            if (busqueda) {
                url += `buscar=${encodeURIComponent(busqueda)}&`;
            }
            if (filtro === 'likes') {
                url += `ordenLikes=desc&`;
            } else if (filtro === 'recientes') {
                url += `ordenFecha=desc&`;
            } else if (filtro === 'antiguas') {
                url += `ordenFecha=asc&`;
            }

            const res = await fetch(url, {
                method: 'GET',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${token}`
                }
            });

            if (!res.ok) throw new Error('Error al obtener las publicaciones');
            
            const data = await res.json();

            setPublicaciones(data);
            
        } catch (err) {
            console.error(err);
            Swal.fire('Error', 'No se pudieron cargar las publicaciones', 'error');
        }
    };

    useEffect(() => {
        const delayBusqueda = setTimeout(() => {
            obtenerPublicaciones();
        }, 300);

        return () => clearTimeout(delayBusqueda);
    }, [busqueda, filtro]);

    const handleEliminar = async (id) => {
        const confirmacion = await Swal.fire({
            title: '¿Eliminar publicación?',
            text: 'Esta acción no se puede deshacer',
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#d33',
            cancelButtonColor: '#0891b2',
            confirmButtonText: 'Sí, eliminar',
            cancelButtonText: 'Cancelar',
        });

        if (!confirmacion.isConfirmed) return;

        try {
            const token = localStorage.getItem('token');

            const res = await fetch(`${API_PUBLICACIONES}/${id}`, {
                method: 'DELETE',
                headers: {
                    'Authorization': `Bearer ${token}`
                }
            });

            if (!res.ok) throw new Error('No se pudo eliminar');

            setPublicaciones(publicaciones.filter((pub) => pub.id_publicacion !== id));

            Swal.fire({
                title: 'Eliminado',
                icon: 'success',
                timer: 1500,
                showConfirmButton: false,
            });
        } catch (err) {
            Swal.fire('Error', err.message, 'error');
        }
    };

    return (
        <div className="p-4">
            <h2 className="text-2xl font-bold text-cyan-800 mb-6">
                Gestión de Publicaciones
            </h2>
            <div className="flex flex-col md:flex-row justify-between items-center mb-8 gap-4 bg-white p-4 rounded-xl shadow-sm">
                
                <input
                    type="text"
                    placeholder="Buscar por título o descripción..."
                    className="border border-gray-300 rounded-lg px-4 py-2 w-full md:w-1/2 focus:outline-none focus:ring-2 focus:ring-cyan-500 transition"
                    value={busqueda}
                    onChange={(e) => setBusqueda(e.target.value)}
                />

                <div className="flex gap-2 w-full md:w-auto overflow-x-auto">
                    <button
                        onClick={() => setFiltro('recientes')}
                        className={`px-4 py-2 rounded-lg font-medium transition ${filtro === 'recientes' ? 'bg-cyan-600 text-white' : 'bg-gray-100 text-gray-600 hover:bg-gray-200'}`}
                    >
                        Recientes
                    </button>
                    <button
                        onClick={() => setFiltro('antiguas')}
                        className={`px-4 py-2 rounded-lg font-medium transition ${filtro === 'antiguas' ? 'bg-cyan-600 text-white' : 'bg-gray-100 text-gray-600 hover:bg-gray-200'}`}
                    >
                        Antiguas
                    </button>
                    <button
                        onClick={() => setFiltro('likes')}
                        className={`px-4 py-2 rounded-lg font-medium transition ${filtro === 'likes' ? 'bg-cyan-600 text-white' : 'bg-gray-100 text-gray-600 hover:bg-gray-200'}`}
                    >
                        Más Likes
                    </button>
                </div>
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                {publicaciones.length === 0 ? (
                    <p className="text-gray-500 col-span-full text-center py-10">No se encontraron publicaciones...</p>
                ) : (
                    publicaciones.map((pub) => (
                        <div
                            key={pub.id_publicacion || pub.id}
                            className="bg-white rounded-2xl shadow-md p-4 relative border border-gray-50 hover:shadow-lg transition flex flex-col h-full"
                        >
                            <button
                                onClick={() => handleEliminar(pub.id_publicacion || pub.id)}
                                className="absolute top-3 right-3 w-8 h-8 flex items-center justify-center rounded-full bg-red-50 hover:bg-red-500 text-red-500 hover:text-white transition shadow-sm z-10"
                                title="Eliminar publicación"
                            >
                                <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2">
                                    <polyline points="3 6 5 6 21 6" />
                                    <path d="M19 6l-1 14a2 2 0 01-2 2H8a2 2 0 01-2-2L5 6" />
                                    <path d="M10 11v6M14 11v6" />
                                    <path d="M9 6V4a1 1 0 011-1h4a1 1 0 011 1v2" />
                                </svg>
                            </button>

                            {pub.contenido ? (
                                <div className="w-full h-40 rounded-lg mb-3 overflow-hidden bg-gray-100 flex-shrink-0">
                                    <img
                                        src={pub.contenido}
                                        alt={pub.titulo}
                                        className="w-full h-full object-cover"
                                        onError={(e) => (e.target.src = 'https://placehold.co/300x200?text=Sin+imagen')}
                                    />
                                </div>
                            ) : (
                                <div className="w-full h-40 rounded-lg mb-3 bg-gray-50 border-2 border-dashed border-gray-200 flex items-center justify-center text-gray-400 flex-shrink-0">
                                    Sin imagen
                                </div>
                            )}
                            <div className="flex-grow">
                                <h3 className="font-bold text-gray-800 pr-8 leading-tight">
                                    {pub.titulo}
                                </h3>

                                {pub.usuarios && (
                                    <p className="text-xs text-cyan-700 font-medium mt-1">
                                        Por: {pub.usuarios.nombre}
                                    </p>
                                )}

                                <p className="text-sm text-gray-500 mt-2 line-clamp-3">
                                    {pub.descripcion}
                                </p>
                            </div>
                            <div className="mt-4 pt-3 border-t border-gray-100 flex justify-between items-center">
                                <span className="text-xs font-semibold text-pink-500 bg-pink-50 px-2 py-1 rounded-md flex items-center gap-1 shadow-sm border border-pink-100">
                                    <svg 
                                        xmlns="http://www.w3.org/2000/svg" 
                                        className="w-3.5 h-3.5" 
                                        viewBox="0 0 20 20" 
                                        fill="currentColor"
                                    >
                                        <path 
                                            fillRule="evenodd" 
                                            d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z" 
                                            clipRule="evenodd" 
                                        />
                                    </svg>
                                    {pub.totalLikes ?? 0}
                                </span>
                                <span className="text-xs text-gray-400">
                                    {new Date(pub.fecha_publicacion).toLocaleDateString()}
                                </span>
                            </div>
                        </div>
                    ))
                )}
            </div>
        </div>
    );
}

export default Publicaciones;


================================================
FILE: src/components/paneles/ModulosAdmin/Usuarios.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";
const API_LOCAL_USUARIOS = "http://localhost:3000";

function Usuarios() {
    const [usuarios, setUsuarios] = useState([]);
    const [filtro, setFiltro] = useState("todos");
    const [busqueda, setBusqueda] = useState("");
    const [rol, setRol] = useState("");
    const [showModalFiltros, setShowModalFiltros] = useState(false);

    const obtenerUsuarios = async () => {
        const token = localStorage.getItem("token");

        try {
            const params = new URLSearchParams();

            if (filtro !== "todos") params.append("estado", filtro);
            if (busqueda.trim() !== "") params.append("buscar", busqueda);
            if (rol !== "") params.append("rol", rol);

            const res = await fetch(
                `${API_LOCAL_USUARIOS}/usuarios?${params.toString()}`,
                {
                    method: "GET",
                    headers: { 
                        "Content-Type": "application/json",
                        Authorization: `Bearer ${token}`,
                    },
                },
            );
            if (res.status === 401) return window.location.reload();
            if (!res.ok) throw new Error("Error al obtener los usuarios");

            const data = await res.json();
            setUsuarios(data);
        } catch (err) {
            console.error(err);
            Swal.fire("Error", "No se pudieron cargar los usuarios", "error");
        }
    };

    useEffect(() => {
        obtenerUsuarios();
    }, [filtro, busqueda, rol]);

    const handleEditar = async (usuario) => {
        const { value: formValues } = await Swal.fire({
            title: '<h3 class="text-2xl font-bold text-cyan-800">Editar Usuario</h3>',
            html: `
            <div class="flex flex-col gap-4 text-left mt-4 px-2">
                <div>
                    <label class="block text-sm font-semibold text-gray-700 mb-1">Nombre</label>
                    <input id="swal-nombre" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 transition-shadow" value="${usuario.nombre}">
                </div>
                
                <div>
                    <label class="block text-sm font-semibold text-gray-700 mb-1">Apellido</label>
                    <input id="swal-apellido" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-cyan-500 focus:border-cyan-500 transition-shadow" value="${usuario.apellido}">
                </div>
                <div class="flex gap-4">
                    <div class="w-1/2">
                        <label class="block text-sm font-semibold text-gray-700 mb-1">Estado</label>
                        <select id="swal-estado" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-cyan-500 bg-white">
                            <option value="true" ${usuario.estado_cuenta ? "selected" : ""}>Activo</option>
                            <option value="false" ${!usuario.estado_cuenta ? "selected" : ""}>Desactivado</option>
                        </select>
                    </div>
                    <div class="w-1/2">
                        <label class="block text-sm font-semibold text-gray-700 mb-1">Rol</label>
                        <select id="swal-rol" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-cyan-500 bg-white">
                            <option value="3" ${usuario.roles?.id_rol === 3 ? "selected" : ""}>Administrador</option>
                            <option value="2" ${usuario.roles?.id_rol === 2 ? "selected" : ""}>Artista</option>
                            <option value="1" ${usuario.roles?.id_rol === 1 ? "selected" : ""}>Usuario Final</option>
                        </select>
                    </div>
                </div>
            </div>
        `,
            focusConfirm: false,
            showCancelButton: true,
            confirmButtonText: "Guardar Cambios",
            cancelButtonText: "Cancelar",
            confirmButtonColor: "#0891b2",
            preConfirm: () => {
                return {
                    nombre: document.getElementById("swal-nombre").value,
                    apellido: document.getElementById("swal-apellido").value,
                    estado_cuenta:
                        document.getElementById("swal-estado").value === "true",
                    id_rol: document.getElementById("swal-rol").value,
                };
            },
        });

        if (formValues) {
            const token = localStorage.getItem("token");

            try {
                const res = await fetch(
                    `${API_LOCAL_USUARIOS}/usuarios/${usuario.id_usuario}`,
                    {
                        method: "PATCH",
                        headers: {
                            "Content-Type": "application/json",
                            Authorization: `Bearer ${token}`,
                        },
                        body: JSON.stringify(formValues),
                    },
                );
                if (res.status === 401) return window.location.reload();
                if (!res.ok)
                    throw new Error("No se pudo actualizar el usuario");
                const usuarioActualizado = await res.json();
                setUsuarios(
                    usuarios.map((u) =>
                        u.id_usuario === usuario.id_usuario
                            ? usuarioActualizado
                            : u,
                    ),
                );
                Swal.fire({
                    title: "Actualizado",
                    icon: "success",
                    timer: 1500,
                    showConfirmButton: false,
                });
            } catch (err) {
                Swal.fire("Error", err.message, "error");
            }
        }
    };

    return (
        <section>
            <div>
                <h2 className="text-2xl font-bold text-cyan-800 mb-6">
                    Usuarios registrados
                </h2>

                <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                    <div className="flex items-center gap-2 w-full md:w-auto">
                        <input
                            type="text"
                            placeholder="Buscar nombre, apellido o email..."
                            value={busqueda}
                            onChange={(e) => setBusqueda(e.target.value)}
                            className="w-full md:w-80 px-4 py-2 text-sm border border-cyan-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-cyan-400 bg-white shadow-sm text-gray-700"
                        />
                        <button
                            onClick={() => setShowModalFiltros(true)}
                            className="px-4 py-2 bg-cyan-600 text-white rounded-xl hover:bg-cyan-700 transition shadow-sm flex items-center gap-2 font-medium text-sm whitespace-nowrap"
                        >
                            <svg
                                xmlns="http://www.w3.org/2000/svg"
                                width="16"
                                height="16"
                                viewBox="0 0 24 24"
                                fill="none"
                                stroke="currentColor"
                                strokeWidth="2"
                                strokeLinecap="round"
                                strokeLinejoin="round"
                            >
                                <path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1-1-1.74v-.5a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"></path>
                                <circle cx="12" cy="12" r="3"></circle>
                            </svg>
                            Filtros
                        </button>
                    </div>
                </div>

                <table className="w-full text-sm rounded-3xl shadow">
                    <thead className="bg-cyan-100 text-cyan-700">
                        <tr>
                            <th className="text-left px-6 py-3">Id</th>
                            <th className="text-left px-6 py-3">Nombre</th>
                            <th className="text-left px-6 py-3">Apellido</th>
                            <th className="text-left px-6 py-3">Email</th>
                            <th className="text-left px-6 py-3">
                                Fecha Registro
                            </th>
                            <th className="text-left px-6 py-3">Estado</th>
                            <th className="text-left px-6 py-3">Rol</th>
                            <th className="text px-6 py-3">Editar</th>
                        </tr>
                    </thead>
                    <tbody>
                        {usuarios.map((u, i) => (
                            <tr
                                key={u.id_usuario}
                                className={
                                    i % 2 === 0 ? "bg-white" : "bg-gray-50"
                                }
                            >
                                <td className="px-6 py-3 font-medium text-gray-700">
                                    {u.id_usuario}
                                </td>
                                <td className="px-6 py-3 text-gray-700">
                                    {u.nombre}
                                </td>
                                <td className="px-6 py-3 text-gray-700">
                                    {u.apellido}
                                </td>
                                <td className="px-6 py-3 text-gray-700">
                                    {u.email}
                                </td>
                                <td className="px-6 py-3 text-gray-700">
                                    {new Date(
                                        u.fecha_registro,
                                    ).toLocaleDateString()}
                                </td>
                                <td className="px-6 py-3">
                                    <span
                                        className={`px-2 py-1 rounded-full text-xs font-medium ${
                                            u.estado_cuenta === true
                                                ? "bg-green-100 text-green-700"
                                                : "bg-red-100 text-red-700"
                                        }`}
                                    >
                                        {u.estado_cuenta === true
                                            ? "Activo"
                                            : "Desactivado"}
                                    </span>
                                </td>
                                <td className="px-6 py-3">
                                    <span
                                        className={`px-2 py-1 rounded-full text-xs font-medium ${
                                            u.roles?.nombre_rol ===
                                            "Administrador"
                                                ? "bg-cyan-100 text-cyan-800"
                                                : u.roles?.nombre_rol === "Artista"
                                                ? "bg-purple-100 text-purple-800"
                                                : "bg-gray-100 text-green-600"
                                        }`}
                                    >
                                        {u.roles?.nombre_rol}
                                    </span>
                                </td>
                                <td className="px-6 py-3 flex gap-3">
                                    <button
                                        onClick={() => handleEditar(u)}
                                        className="text-blue-500 hover:text-blue-700 transition"
                                        title="Editar usuario"
                                    >
                                        <svg
                                            width="18"
                                            height="18"
                                            viewBox="0 0 24 24"
                                            fill="none"
                                            stroke="currentColor"
                                            strokeWidth="2"
                                        >
                                            <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7" />
                                            <path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z" />
                                        </svg>
                                    </button>
                                </td>
                            </tr>
                        ))}
                    </tbody>
                </table>
            </div>

            {showModalFiltros && (
                <div
                    className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm"
                    onClick={() => setShowModalFiltros(false)}
                >
                    <div
                        className="bg-white rounded-2xl p-6 w-full max-w-sm mx-4 shadow-xl"
                        onClick={(e) => e.stopPropagation()}
                    >
                        <div className="flex justify-between items-center mb-4">
                            <h3 className="text-lg font-bold text-gray-700">
                                Filtros Avanzados
                            </h3>
                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="text-gray-400 hover:text-gray-600 font-bold text-lg"
                            >
                                ✕
                            </button>
                        </div>

                        <div className="flex flex-col gap-4">
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">
                                    Estado de la cuenta:
                                </label>
                                <select
                                    value={filtro}
                                    onChange={(e) => setFiltro(e.target.value)}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="todos">
                                        Todos los usuarios
                                    </option>
                                    <option value="true">Solo Activos</option>
                                    <option value="false">
                                        Solo Desactivados
                                    </option>
                                </select>
                            </div>

                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">
                                    Rol de usuario:
                                </label>
                                <select
                                    value={rol}
                                    onChange={(e) => setRol(e.target.value)}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="">Todos los roles</option>
                                    <option value="3">Administradores</option>
                                    <option value="2">Artistas</option>
                                    <option value="1">Usuarios Finales</option>
                                </select>
                            </div>

                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="mt-2 w-full bg-cyan-600 hover:bg-cyan-700 text-white font-medium py-2 rounded-xl text-sm transition"
                            >
                                Aplicar filtros
                            </button>
                        </div>
                    </div>
                </div>
            )}
        </section>
    );
}

export default Usuarios;



================================================
FILE: src/components/paneles/ModulosAdmin/VistaUsuario.jsx
================================================
import { useState, useEffect } from "react";
import Swal from "sweetalert2";
import PublicacionesCom from "../../PublicacionesCom";

const API_LOCAL_BACKEND = "http://localhost:3000";

function VistaUsuario() {
    const usuario = JSON.parse(localStorage.getItem("usuario"));
    const [publicaciones, setPublicaciones] = useState([]);
    const [loading, setLoading] = useState(true);
    const [busqueda, setBusqueda] = useState("");
    const [filtroFecha, setFiltroFecha] = useState("desc");
    const [filtroLikes, setFiltroLikes] = useState("");
    const [showModalFiltros, setShowModalFiltros] = useState(false);

    const obtenerPublicacionesMuro = async () => {
        const token = localStorage.getItem("token");

        try {
            setLoading(true);

            let url = `${API_LOCAL_BACKEND}/Muro-Publicaciones?buscar=${encodeURIComponent(busqueda)}&ordenFecha=${filtroFecha}`;
            if (filtroLikes) {
                url += `&ordenLikes=${filtroLikes}`;
            }

            const res = await fetch(url, {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                    Authorization: `Bearer ${token}`,
                },
            });

            if (res.status === 401) {
                console.log("Sesión expirada o token inválido");
                localStorage.removeItem("token");
                localStorage.removeItem("usuario");

                Swal.fire({
                    icon: "warning",
                    title: "Sesión Expirada",
                    text: "Tu sesión de administrador ha terminado por seguridad. Vuelve a ingresar.",
                    confirmButtonColor: "#0891b2",
                }).then(() => {
                    window.location.reload();
                });
                return;
            }

            if (!res.ok)
                throw new Error("Error al obtener las publicaciones del muro");

            const data = await res.json();
            setPublicaciones(data);
        } catch (err) {
            console.error(err);
            Swal.fire(
                "Error",
                "No se pudieron cargar las obras del muro",
                "error",
            );
        } finally {
            setLoading(false);
        }
    };

    useEffect(() => {
        obtenerPublicacionesMuro();
    }, [busqueda, filtroFecha, filtroLikes]);

    return (
        <div className="w-full flex flex-col">
            <div className="flex-1 w-full">
                <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                    <div>
                        <h1 className="text-2xl font-bold text-cyan-800 mb-1 flex items-center gap-2">
                            <svg
                                className="w-6 h-6"
                                fill="none"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                                strokeWidth={2}
                                strokeLinecap="round"
                                strokeLinejoin="round"
                            >
                                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z" />
                                <circle cx="12" cy="12" r="3" />
                            </svg>
                            Modo Vista de Usuario
                        </h1>
                        <p className="text-gray-500 text-sm">
                            Visualizando e interactuando con el muro tal como lo
                            ven los usuarios. (Admin: {usuario?.nombre})
                        </p>
                    </div>

                    <div className="flex items-center gap-2 w-full md:w-96">
                        <input
                            type="text"
                            placeholder="Buscar por título o descripción..."
                            value={busqueda}
                            onChange={(e) => setBusqueda(e.target.value)}
                            className="w-full px-4 py-2 text-sm border border-cyan-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-cyan-400 bg-white shadow-sm text-gray-700"
                        />
                        <button
                            onClick={() => setShowModalFiltros(true)}
                            className="px-3 py-2 bg-cyan-600 text-white rounded-xl hover:bg-cyan-700 transition shadow-sm flex items-center gap-1 font-medium text-sm whitespace-nowrap"
                        >
                            <svg
                                xmlns="http://www.w3.org/2000/svg"
                                width="16"
                                height="16"
                                viewBox="0 0 24 24"
                                fill="none"
                                stroke="currentColor"
                                strokeWidth="2"
                                strokeLinecap="round"
                                strokeLinejoin="round"
                            >
                                <path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1-1-1.74v-.5a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"></path>
                                <circle cx="12" cy="12" r="3"></circle>
                            </svg>
                            Filtros
                        </button>
                    </div>
                </div>

                <div className="bg-white rounded-2xl shadow p-6 border border-gray-100">
                    <h2 className="text-lg font-bold text-cyan-700 mb-4">
                        Muro de Obras
                    </h2>

                    {loading && (
                        <p className="text-center text-gray-400 py-10">
                            Cargando publicaciones...
                        </p>
                    )}

                    {!loading && publicaciones.length === 0 && (
                        <p className="text-center text-gray-400 py-10">
                            No se encontraron obras que coincidan con los
                            filtros aplicados.
                        </p>
                    )}

                    {!loading && publicaciones.length > 0 && (
                        <div className="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6">
                            {publicaciones.map((item) => (
                                <PublicacionesCom
                                    key={item.id_publicacion || item.id}
                                    item={item}
                                />
                            ))}
                        </div>
                    )}
                </div>
            </div>

            {showModalFiltros && (
                <div
                    className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm"
                    onClick={() => setShowModalFiltros(false)}
                >
                    <div
                        className="bg-white rounded-2xl p-6 w-full max-w-sm mx-4 shadow-xl"
                        onClick={(e) => e.stopPropagation()}
                    >
                        <div className="flex justify-between items-center mb-4">
                            <h3 className="text-lg font-bold text-gray-700">
                                Filtros Avanzados
                            </h3>
                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="text-gray-400 hover:text-gray-600 font-bold text-lg"
                            >
                                ✕
                            </button>
                        </div>

                        <div className="flex flex-col gap-4">
                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">
                                    Ordenar por Fecha:
                                </label>
                                <select
                                    value={filtroFecha}
                                    onChange={(e) => {
                                        setFiltroFecha(e.target.value);
                                        setFiltroLikes("");
                                    }}
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="desc">
                                        Más recientes primero
                                    </option>
                                    <option value="asc">
                                        Más antiguas primero
                                    </option>
                                </select>
                            </div>

                            <div>
                                <label className="text-xs font-semibold text-gray-500 block mb-1">
                                    Ordenar por Popularidad (Likes):
                                </label>
                                <select
                                    value={filtroLikes}
                                    onChange={(e) =>
                                        setFiltroLikes(e.target.value)
                                    }
                                    className="w-full p-2 border border-gray-200 rounded-xl text-sm focus:ring-2 focus:ring-cyan-400 focus:outline-none bg-white text-gray-700"
                                >
                                    <option value="">
                                        Por defecto (Sin aplicar filtro de
                                        likes)
                                    </option>
                                    <option value="desc">
                                        Más populares (Mayor a menor)
                                    </option>
                                    <option value="asc">
                                        Menos populares (Menor a mayor)
                                    </option>
                                </select>
                            </div>

                            <button
                                onClick={() => setShowModalFiltros(false)}
                                className="mt-2 w-full bg-cyan-600 hover:bg-cyan-700 text-white font-medium py-2 rounded-xl text-sm transition"
                            >
                                Aplicar filtros
                            </button>
                        </div>
                    </div>
                </div>
            )}
        </div>
    );
}

export default VistaUsuario;



================================================
FILE: src/components/paneles/ModulosConversaciones/Chat.jsx
================================================
import { useState, useEffect } from "react";

const API = "http://localhost:3000";

function Chat({ usuario, chatActivo, nombreChat }) {
    const [mensajes, setMensajes] = useState([]);
    const [texto, setTexto] = useState("");
    const usuarioId = usuario?.id_usuario || usuario?.id;

    const cargarMensajes = () => {
        const token = localStorage.getItem("token");
        if (!chatActivo) return;
        fetch(`${API}/mensajes/${chatActivo}`, {
            method: "GET",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${token}`,
            },
        })
            .then((res) => res.json())
            .then((data) => setMensajes(data || []))
            .catch((err) => console.error("Error al cargar mensajes:", err));
    };

    useEffect(() => {
        if (!chatActivo) return;
        cargarMensajes();

        const intervalo = setInterval(cargarMensajes, 3000);
        return () => clearInterval(intervalo);
    }, [chatActivo]);

    const enviarMensaje = () => {
        const token = localStorage.getItem("token");
        if (!texto.trim() || !usuarioId) return;

        fetch(`${API}/mensajes`, {
            method: "POST",
            headers: { "Content-Type": "application/json", Authorization: `Bearer ${token}`},
            body: JSON.stringify({
                id_conversacion: chatActivo,
                id_usuario: usuarioId,
                contenido: texto.trim(),
            }),
        })
            .then((res) => {
                if (!res.ok) throw new Error("No se pudo enviar el mensaje");
                return res.json();
            })
            .then(() => {
                setTexto("");
                cargarMensajes();
            })
            .catch((err) => console.error(err));
    };

    return (
        <div className="flex flex-col h-full">
            <div className="px-5 py-3 border-b border-gray-100 flex items-center gap-3">
                <div className="w-9 h-9 rounded-full bg-cyan-100 flex items-center justify-center text-cyan-700 font-bold">
                    {nombreChat?.charAt(0).toUpperCase() || "?"}
                </div>
                <p className="font-semibold text-gray-700">{nombreChat}</p>
            </div>

            <div className="flex-1 overflow-y-auto px-5 py-4 flex flex-col gap-2 bg-slate-50">
                {mensajes.length === 0 ? (
                    <p className="text-center text-gray-400 text-sm mt-10">
                        Aún no hay mensajes. ¡Di hola!
                    </p>
                ) : (
                    mensajes.map((m) => {
                        const esMio =
                            String(m.id_usuario) === String(usuarioId);
                        return (
                            <div
                                key={m.id_mensaje || m.id}
                                className={`flex ${esMio ? "justify-end" : "justify-start"}`}
                            >
                                <div
                                    className={`max-w-xs px-4 py-2 rounded-2xl text-sm ${
                                        esMio
                                            ? "bg-cyan-600 text-white rounded-br-sm shadow-sm"
                                            : "bg-white text-gray-700 rounded-bl-sm border border-gray-100 shadow-sm"
                                    }`}
                                >
                                    <p>{m.contenido}</p>
                                    <p
                                        className={`text-[10px] mt-1 text-right ${
                                            esMio
                                                ? "text-cyan-200"
                                                : "text-gray-400"
                                        }`}
                                    >
                                        {m.fecha_envio
                                            ? new Date(
                                                m.fecha_envio,
                                            ).toLocaleTimeString([], {
                                                hour: "2-digit",
                                                minute: "2-digit",
                                            })
                                            : "Reciente"}
                                    </p>
                                </div>
                            </div>
                        );
                    })
                )}
            </div>

            <div className="px-4 py-3 border-t border-gray-100 flex gap-2 bg-white">
                <input
                    type="text"
                    value={texto}
                    onChange={(e) => setTexto(e.target.value)}
                    onKeyDown={(e) => e.key === "Enter" && enviarMensaje()}
                    placeholder="Escribe un mensaje..."
                    className="flex-1 text-black border border-gray-200 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-300"
                />
                <button
                    onClick={enviarMensaje}
                    className="bg-cyan-600 hover:bg-cyan-700 text-white px-4 py-2 rounded-xl transition flex items-center justify-center"
                >
                    <svg
                        width="16"
                        height="16"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        strokeWidth="2"
                    >
                        <line x1="22" y1="2" x2="11" y2="13" />
                        <polygon points="22 2 15 22 11 13 2 9 22 2" />
                    </svg>
                </button>
            </div>
        </div>
    );
}

export default Chat;



================================================
FILE: src/components/paneles/ModulosConversaciones/CrearConversacion.jsx
================================================
import { useState } from "react";
import Swal from "sweetalert2";

const API = "http://localhost:3000";

function BuscarUsuario({ usuario, onChatIniciado }) {
    const [correo, setCorreo] = useState("");
    const [resultado, setResultado] = useState(null);
    const [mensaje, setMensaje] = useState("");
    const usuarioId = usuario?.id_usuario || usuario?.id;

    const buscar = () => {
        const token = localStorage.getItem("token");
        if (!correo.trim()) return;

        fetch(`${API}/usuarios/buscar?email=${correo.trim()}`, {
            method: "GET",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${token}`,
            },
        })
            .then((res) => {
                if (!res.ok) throw new Error("Error en la búsqueda");
                return res.json();
            })
            .then((data) => {
                if (!data) {
                    setResultado(null);
                    setMensaje("No se encontró ningún usuario con ese correo.");
                } else if (
                    String(data.id_usuario || data.id) === String(usuarioId)
                ) {
                    setResultado(null);
                    setMensaje("Ese eres tú 😄");
                } else {
                    setResultado(data);
                    setMensaje("");
                }
            })
            .catch((err) => {
                console.error(err);
                setResultado(null);
                setMensaje("Ocurrió un error al buscar el usuario.");
            });
    };

    const iniciarChat = () => {
        const token = localStorage.getItem("token");
        const targetId = resultado?.id_usuario || resultado?.id;

        if (!usuarioId || !targetId) return;

        fetch(`${API}/conversaciones`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${token}`,
            },
            body: JSON.stringify({
                id_usuario_1: usuarioId,
                id_usuario_2: targetId,
            }),
        })
            .then((res) => {
                if (!res.ok)
                    throw new Error("No se pudo iniciar la conversación");
                return res.json();
            })
            .then((data) => {
                const nombreOtro =
                    `${resultado.nombre} ${resultado.apellido || ""}`.trim();
                if (data.existe) {
                    Swal.fire({
                        title: "Chat ya existe",
                        text: `Ya tienes una conversación con ${nombreOtro}`,
                        icon: "info",
                        confirmButtonColor: "#0891b2",
                        timer: 2000,
                        showConfirmButton: false,
                    });
                } else {
                    Swal.fire({
                        title: "¡Chat creado!",
                        text: `Ahora puedes chatear con ${nombreOtro}`,
                        icon: "success",
                        confirmButtonColor: "#0891b2",
                        timer: 2000,
                        showConfirmButton: false,
                    });
                }
                limpiar();
                onChatIniciado(data.id_conversacion, nombreOtro);
            })
            .catch((err) => {
                console.error(err);
                Swal.fire("Error", "No se pudo iniciar el chat.", "error");
            });
    };

    const limpiar = () => {
        setCorreo("");
        setResultado(null);
        setMensaje("");
    };

    return (
        <div className="bg-white rounded-2xl shadow p-6 max-w-md">
            <h2 className="text-lg font-bold text-cyan-700 mb-4">Nuevo chat</h2>

            <div className="flex gap-2 mb-4">
                <input
                    type="text"
                    placeholder="Buscar por correo..."
                    value={correo}
                    onChange={(e) => setCorreo(e.target.value)}
                    onKeyDown={(e) => e.key === "Enter" && buscar()}
                    className="flex-1 border border-gray-200 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-cyan-300"
                />
                <button
                    onClick={buscar}
                    className="bg-cyan-600 hover:bg-cyan-700 text-white px-4 py-2 rounded-xl text-sm transition cursor-pointer"
  