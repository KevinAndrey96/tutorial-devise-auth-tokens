# Guía de Autenticación en Ruby on Rails con Devise

[![Ruby](https://img.shields.io/badge/Ruby-2.x-CC342D?style=flat&logo=ruby&logoColor=white)](https://www.ruby-lang.org/)
[![Rails](https://img.shields.io/badge/Rails-4.x%20%2F%205.x-CC0000?style=flat&logo=rubyonrails&logoColor=white)](https://rubyonrails.org/)
[![Devise](https://img.shields.io/badge/Gem-Devise-brightgreen?style=flat)](https://github.com/heartcombo/devise)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Guía paso a paso y aplicación de referencia para la implementación de autenticación de usuarios en **Ruby on Rails** utilizando la gema **Devise**.

Devise es la solución estándar en el ecosistema Rails para la gestión completa del ciclo de vida de usuarios, basada en arquitectura modular sobre **Warden** y el middleware de Rack.

---

## 🏛️ Módulos Principales de Devise

Devise organiza sus capacidades en 10 módulos independientes configurables en el modelo `User`:

* **Database Authenticatable:** Hashea y almacena contraseñas seguras en la base de datos (bcrypt).
* **Registerable:** Gestiona el registro de nuevos usuarios y edición/eliminación de cuentas.
* **Recoverable:** Restablecimiento de contraseñas olvidadas mediante tokens seguros por correo electrónico.
* **Rememberable:** Generación de cookies persistentes (*Remember me*).
* **Trackable:** Auditoría de inicios de sesión (conteo de accesos, marcas de tiempo e IPs).
* **Validatable:** Validaciones de formato de email y longitud mínima de contraseñas.

---

## 🔐 Protección de Controladores

Para restringir el acceso a controladores específicos y forzar que solo usuarios con sesión iniciada puedan interactuar con ellos, se utiliza el filtro `before_action`:

```ruby
# app/controllers/secret_controller.rb
class SecretController < ApplicationController
  # Intercepta cualquier acción y redirige al login si no hay sesión activa
  before_action :authenticate_user!

  def index
    # Solo accesible por usuarios autenticados
  end
end
```

### Helpers Globales de Vista y Controlador

```erb
<% if user_signed_in? %>
  <span>Bienvenido, <%= current_user.email %></span>
  <%= link_to "Cerrar sesión", destroy_user_session_path, method: :delete %>
<% else %>
  <%= link_to "Iniciar sesión", new_user_session_path %>
  <%= link_to "Registrarse", new_user_registration_path %>
<% end %>
```

---

## 📂 Estructura del Proyecto

```
tutorial-devise-auth-tokens/
├── app/
│   ├── controllers/
│   │   ├── application_controller.rb
│   │   ├── home_controller.rb         # Ruta pública de bienvenida (/)
│   │   └── secret_controller.rb       # Ruta protegida con :authenticate_user!
│   ├── models/
│   │   └── user.rb                    # Modelo de usuario con módulos Devise
│   └── views/
│       ├── home/index.html.erb        # Vista pública
│       └── secret/index.html.erb      # Vista secreta para usuarios autenticados
├── config/
│   ├── routes.rb                      # devise_for :users y rutas de la app
│   └── initializers/devise.rb         # Configuración global de la gema Devise
├── db/
│   └── migrate/                       # Migración de tabla users
├── Gemfile
└── README.md
```

---

## 🚀 Instalación y Puesta en Marcha

### 1. Clonar e Instalar Gemas
```bash
git clone git@github.com:KevinAndrey96/tutorial-devise-auth-tokens.git
cd tutorial-devise-auth-tokens

bundle install
```

### 2. Configurar Base de Datos y Migraciones
```bash
rake db:create
rake db:migrate
```

### 3. Iniciar Servidor de Desarrollo
```bash
rails server
```
Abre tu navegador en `http://localhost:3000`.

---

## 🛠️ Comandos Esenciales de Devise

| Tarea | Comando Rails |
|---|---|
| Instalar configuración inicial de Devise | `rails generate devise:install` |
| Generar modelo de usuario | `rails generate devise User` |
| Exportar vistas para personalización | `rails generate devise:views` |
| Listar todas las rutas generadas por Devise | `rake routes \| grep user` |

---

## 📄 Licencia
Distribuido bajo licencia [MIT](LICENSE).

**Autor:** [Kevin Andrey Herrera Silva](https://github.com/KevinAndrey96) · Bogotá, Colombia
