<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>𝐒𝐎𝐌𝐁𝐑𝐀_𝐌𝐎𝐃𝐙</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',sans-serif}
        :root{--fondo:#0a0a0a;--dorado:#d4af37;--verde:#00ff88;--rojo:#ff3333;--gris:#1a1a1a}
        body{background:var(--fondo);color:#fff;min-height:100vh}
        .cabecera{text-align:center;padding:30px 20px;border-bottom:2px solid var(--dorado)}
        .cabecera h1{font-size:2.5em;letter-spacing:3px;color:var(--dorado);text-shadow:0 0 15px var(--dorado)}
        .contenedor{max-width:1100px;margin:0 auto;padding:30px 20px}
        .caja{background:var(--gris);border-radius:12px;padding:25px;margin-bottom:25px;border:1px solid #333;box-shadow:0 0 12px rgba(212,175,55,.2)}
        .caja h2{color:var(--dorado);margin-bottom:20px}
        .btn{padding:12px 25px;border:none;border-radius:8px;font-size:1em;font-weight:bold;cursor:pointer;transition:.3s;margin:5px}
        .btn-dorado{background:linear-gradient(45deg,#b8962c,var(--dorado));color:#000}
        .btn-verde{background:var(--verde);color:#000}
        .btn-rojo{background:var(--rojo);color:#fff}
        .btn:hover{transform:scale(1.05);box-shadow:0 0 12px currentColor}
        input,select{width:100%;padding:12px;margin:8px 0;background:#2a2a2a;border:1px solid #444;border-radius:8px;color:#fff;font-size:1em}
        .oculto{display:none !important}
        .clave-generada{background:#0f2f1f;padding:15px;border-radius:8px;border:1px solid var(--verde);margin-top:15px;word-break:break-all;font-family:monospace;font-size:1.1em;color:var(--verde)}
        .lista-claves{margin-top:15px}
        .clave-item{background:#222;padding:12px;border-radius:6px;margin:8px 0;display:flex;justify-content:space-between;align-items:center;border-left:3px solid var(--dorado)}
        .estado-activa{color:var(--verde)}
        .estado-usada{color:var(--rojo)}
        .aviso{padding:12px;border-radius:8px;margin:10px 0}
        .exito{background:rgba(0,255,136,.15);border:1px solid var(--verde)}
        .error{background:rgba(255,51,51,.15);border:1px solid var(--rojo)}
        .pie{text-align:center;padding:20px;color:#777;border-top:1px solid #222;margin-top:30px}
        .pestanas{display:flex;gap:10px;margin-bottom:20px;flex-wrap:wrap}
        .pestana{padding:10px 20px;background:#2a2a2a;border-radius:8px;cursor:pointer;border:1px solid transparent}
        .pestana.activa{border-color:var(--dorado);color:var(--dorado);background:#252525}
        .tarjeta-panel{background:#252525;padding:20px;border-radius:10px;border:1px solid #444;text-align:center;cursor:pointer;transition:.3s;margin:10px;flex:1;min-width:150px}
        .tarjeta-panel:hover{border-color:var(--dorado);transform:translateY(-3px)}
        .fila-paneles{display:flex;flex-wrap:wrap;gap:10px;margin:15px 0}
        .codigo-copiable{cursor:pointer;user-select:all}
        .ayuda{font-size:.85em;color:#888;margin-top:-5px;margin-bottom:8px}
    </style>
</head>
<body>

<div class="cabecera">
    <h1>𝐒𝐎𝐌𝐁𝐑𝐀_𝐌𝐎𝐃𝐙</h1>
    <p>Sistema de Claves Oficial</p>
</div>

<div class="contenedor">
    <!-- PANEL DE LOGIN ADMIN -->
    <div id="caja-login-admin" class="caja">
        <h2>🔐 Administración</h2>
        <input type="password" id="pass-admin" placeholder="Contraseña de Administrador">
        <button class="btn btn-dorado" onclick="verificarAdmin()">Ingresar al Panel</button>
        <p style="margin-top:10px;color:#888;font-size:.9em;">Solo para el dueño</p>
    </div>

    <!-- PANEL ADMIN COMPLETO -->
    <div id="panel-admin" class="oculto">
        <div class="pestanas">
            <div class="pestana activa" onclick="cambiarPestana('paneles')">Paneles</div>
            <div class="pestana" onclick="cambiarPestana('agregar')">Generar Clave</div>
            <div class="pestana" onclick="cambiarPestana('lista')">Mis Claves</div>
            <div class="pestana" onclick="cambiarPestana('verificar')">Verificar</div>
            <div class="pestana" onclick="cerrarSesion()">Salir</div>
        </div>

        <div id="p-paneles" class="caja">
            <h2>📁 Paneles Disponibles</h2>
            <p style="color:#aaa;margin-bottom:15px;">Aquí agregas los paneles que la gente verá</p>
            <div class="fila-paneles" id="lista-paneles"></div>
            <h3 style="margin-top:25px;color:var(--dorado)">➕ Agregar Nuevo Panel</h3>
            <input type="text" id="nuevo-panel-nombre" placeholder="Nombre del panel">
            <button class="btn btn-verde" onclick="agregarPanel()">Agregar Panel</button>
        </div>

        <!-- CREAR CLAVE — AHORA CON PREFIJO PERSONALIZADO -->
        <div id="p-agregar" class="caja">
            <h2>➕ Crear Clave Personalizada</h2>
            
            <label>Panel:</label>
            <select id="clave-panel"></select>
            
            <label>Tipo de Clave / Prefijo:</label>
            <input type="text" id="prefijo-clave" placeholder="Ejemplo: Key-v###" value="Key-v###">
            <p class="ayuda">Usa ### para que salgan números aleatorios → Ej: Key-v150</p>
            
            <label>Duración:</label>
            <select id="duracion-clave">
                <option value="1">1 Día</option>
                <option value="7">7 Días</option>
                <option value="30">30 Días</option>
                <option value="365">1 Año</option>
                <option value="0" selected>Permanente</option>
            </select>
            
            <button class="btn btn-verde" onclick="generarNuevaClave()">Crear Clave</button>
            <div id="resultado-clave" class="clave-generada oculto"></div>
        </div>

        <div id="p-lista" class="caja">
            <h2>📋 Todas las Claves</h2>
            <div id="lista-todas-claves" class="lista-claves"></div>
        </div>

        <div id="p-verificar" class="caja">
            <h2>🔍 Revisar Clave</h2>
            <input type="text" id="clave-verificar" placeholder="Escribe la clave a revisar">
            <button class="btn btn-dorado" onclick="revisarClave()">Comprobar</button>
            <div id="resultado-verificacion"></div>
        </div>
    </div>

    <!-- ZONA PÚBLICA -->
    <div id="zona-publica" class="caja">
        <h2>🎁 Elegir Panel</h2>
        <p style="color:#aaa;margin-bottom:20px;">Selecciona el panel y obtén tu clave</p>
        <label>Elige tu panel:</label>
        <select id="panel-usuario" onchange="cargarClavesDisponibles()">
            <option value="">-- Seleccionar --</option>
        </select>
        <div id="claves-disponibles" style="margin-top:20px;"></div>
    </div>
</div>

<div class="pie">© 2026 𝐒𝐎𝐌𝐁𝐑𝐀_𝐌𝐎𝐃𝐙</div>

<script>
const PASS_ADMIN = "7233739.Smb";
const STORAGE_DATOS = "sombra_modz_datos";

let datos = { paneles: ["FIXERS XITERS"], claves: [] };

async function iniciar(){
    const guardado = localStorage.getItem(STORAGE_DATOS);
    if(guardado) datos = JSON.parse(guardado);
    actualizarSelectores();
    renderizarPaneles();
}

async function sincronizar(){
    localStorage.setItem(STORAGE_DATOS, JSON.stringify(datos));
}

iniciar();

// === LOGIN ===
function verificarAdmin(){
    if(document.getElementById("pass-admin").value === PASS_ADMIN){
        document.getElementById("caja-login-admin").classList.add("oculto");
        document.getElementById("panel-admin").classList.remove("oculto");
        actualizarListaClaves();
    }else alert("❌ Contraseña incorrecta");
}

function cerrarSesion(){
    document.getElementById("panel-admin").classList.add("oculto");
    document.getElementById("caja-login-admin").classList.remove("oculto");
    document.getElementById("pass-admin").value = "";
}

function cambiarPestana(nombre){
    document.querySelectorAll(".pestana").forEach(p => p.classList.remove("activa"));
    event.target.classList.add("activa");
    ["paneles","agregar","lista","verificar"].forEach(p => {
        document.getElementById(`p-${p}`).classList.toggle("oculto", p!==nombre);
    });
    if(nombre==="paneles") renderizarPaneles();
    if(nombre==="lista") actualizarListaClaves();
}

// === PANELES ===
async function agregarPanel(){
    const nombre = document.getElementById("nuevo-panel-nombre").value.trim();
    if(!nombre){alert("Escribe un nombre");return;}
    if(datos.paneles.includes(nombre)){alert("Ya existe ese panel");return;}
    datos.paneles.push(nombre);
    await sincronizar();
    document.getElementById("nuevo-panel-nombre").value = "";
    actualizarSelectores();
    renderizarPaneles();
}

function renderizarPaneles(){
    const caja = document.getElementById("lista-paneles");
    caja.innerHTML = datos.paneles.map(p => `
        <div class="tarjeta-panel">
            <h4>${p}</h4>
            <small>${datos.claves.filter(c=>c.panel===p).length} claves</small>
            <br>
            <button class="btn btn-rojo" style="padding:5px 10px;font-size:.8em;margin-top:8px" onclick="eliminarPanel('${p}')">Eliminar</button>
        </div>
    `).join("");
}

async function eliminarPanel(nombre){
    if(!confirm(`¿Eliminar panel "${nombre}"?`)) return;
    datos.paneles = datos.paneles.filter(p => p !== nombre);
    datos.claves = datos.claves.filter(c => c.panel !== nombre);
    await sincronizar();
    actualizarSelectores();
    renderizarPaneles();
}

// === GENERAR CLAVE PERSONALIZADA ===
function crearCodigoClave(prefijo){
    // Reemplaza ### por números aleatorios
    let codigo = prefijo;
    while(codigo.includes("###")){
        const num = Math.floor(Math.random() * 999) + 1; // 1-999
        const numStr = String(num).padStart(3, '0'); // 001, 015, 150
        codigo = codigo.replace("###", numStr);
    }
    // Si no tiene ###, se queda tal cual
    return codigo;
}

async function generarNuevaClave(){
    const panel = document.getElementById("clave-panel").value;
    const prefijo = document.getElementById("prefijo-clave").value.trim() || "Key-v###";
    const dias = parseInt(document.getElementById("duracion-clave").value);
    
    if(!panel){alert("Selecciona un panel");return;}
    
    const idGenerado = crearCodigoClave(prefijo);
    
    // Verificar que no se repita
    if(datos.claves.some(c => c.id === idGenerado)){
        alert("⚠️ Esa clave ya existe, cambia el prefijo");
        return;
    }
    
    const nueva = {
        id: idGenerado,
        panel: panel,
        fecha_creacion: new Date().toISOString(),
        expira: dias===0 ? null : new Date(Date.now() + dias*86400000).toISOString(),
        estado: "disponible"
    };
    
    datos.claves.push(nueva);
    await sincronizar();
    
    const caja = document.getElementById("resultado-clave");
    caja.classList.remove("oculto");
    caja.innerHTML = `✅ Clave Creada:<br>
        <strong class="codigo-copiable" onclick="navigator.clipboard.writeText('${nueva.id}')">${nueva.id}</strong><br>
        Panel: ${panel}<br>
        Validez: ${dias===0?"Permanente":dias+" días"}<br>
        <small style="color:#888;margin-top:8px;display:block;">Toca para copiar</small>`;
}

// === USUARIO OBTIENE CLAVE ===
function cargarClavesDisponibles(){
    const panelElegido = document.getElementById("panel-usuario").value;
    const caja = document.getElementById("claves-disponibles");
    if(!panelElegido){caja.innerHTML="";return;}
    
    const disponibles = datos.claves.filter(c => 
        c.panel === panelElegido && c.estado === "disponible" &&
        (!c.expira || new Date(c.expira) >= new Date())
    );
    
    if(disponibles.length === 0){
        caja.innerHTML = `<div class="aviso error">⚠️ No hay claves disponibles para este panel</div>`;
        return;
    }
    
    caja.innerHTML = `
        <p style="color:var(--verde)">✅ ${disponibles.length} clave(s) disponible(s)</p>
        <button class="btn btn-verde" onclick="entregarClave('${panelElegido}')">Obtener Mi Clave</button>
    `;
}

async function entregarClave(panelNombre){
    const clave = datos.claves.find(c => 
        c.panel === panelNombre && c.estado === "disponible" &&
        (!c.expira || new Date(c.expira) >= new Date())
    );
    
    if(!clave){
        alert("⚠️ Ya no hay claves disponibles");
        cargarClavesDisponibles();
        return;
    }
    
    clave.estado = "entregada";
    await sincronizar();
    
    const caja = document.getElementById("claves-disponibles");
    caja.innerHTML = `
        <div class="clave-generada">
            ✅ Tu clave es:<br>
            <strong class="codigo-copiable" onclick="navigator.clipboard.writeText('${clave.id}')">${clave.id}</strong><br>
            Panel: ${clave.panel}<br>
            Expira: ${clave.expira?new Date(clave.expira).toLocaleDateString():"Nunca"}
        </div>
    `;
}

// === VERIFICAR ===
function revisarClave(){
    const busqueda = document.getElementById("clave-verificar").value.trim().toUpperCase();
    const encontrada = datos.claves.find(c => c.id === busqueda);
    const res = document.getElementById("resultado-verificacion");
    
    if(!encontrada){
        res.innerHTML = `<div class="aviso error">❌ Clave NO existe</div>`;
        return;
    }
    
    let valido = true, mensaje = "✅ AUTORIZADO";
    if(encontrada.estado === "disponible"){mensaje="⚠️ Clave aún no asignada";valido=false;}
    else if(encontrada.estado === "usada"){mensaje="❌ Ya fue utilizada";valido=false;}
    else if(encontrada.expira && new Date(encontrada.expira) < new Date()){mensaje="⏰ Expirada";valido=false;}
    
    res.innerHTML = `
        <div class="aviso ${valido?'exito':'error'}">
            Clave: ${encontrada.id}<br>
            Panel: ${encontrada.panel}<br>
            Estado: ${mensaje}<br>
            Expira: ${encontrada.expira?new Date(encontrada.expira).toLocaleDateString():"Nunca"}
        </div>
    `;
}

// === LISTA ===
function actualizarListaClaves(){
    const caja = document.getElementById("lista-todas-claves");
    if(datos.claves.length===0){
        caja.innerHTML = "<p style='color:#777'>Sin claves creadas</p>";
        return;
    }
    
    caja.innerHTML = datos.claves.map((c,i) => {
        let clase = "estado-activa", texto = c.estado;
        if(c.estado==="disponible"){clase="";texto="✅ Disponible";}
        else if(c.estado==="entregada"){clase="estado-activa";texto="🔑 Entregada";}
        else if(c.estado==="usada"){clase="estado-usada";texto="❌ Usada";}
        else if(c.expira && new Date(c.expira)<new Date()){clase="estado-usada";texto="⏰ Vencida";}
        
        return `
        <div class="clave-item">
            <div>
                <strong>${c.id}</strong><br>
                <small>${c.panel} · ${new Date(c.fecha_creacion).toLocaleDateString()}</small>
            </div>
            <div>
                <span class="${clase}">${texto}</span>
                <button class="btn btn-rojo" style="padding:5px 10px;font-size:.8em" onclick="eliminarClave(${i})">✕</button>
            </div>
        </div>`;
    }).join("");
}

async function eliminarClave(indice){
    if(confirm("¿Eliminar esta clave?")){
        datos.claves.splice(indice,1);
        await sincronizar();
        actualizarListaClaves();
        cargarClavesDisponibles();
    }
}

function actualizarSelectores(){
    const selAdmin = document.getElementById("clave-panel");
    selAdmin.innerHTML = datos.paneles.map(p => `<option value="${p}">${p}</option>`).join("");
    
    const selPub = document.getElementById("panel-usuario");
    const actual = selPub.value;
    selPub.innerHTML = `<option value="">-- Seleccionar --</option>` + 
        datos.paneles.map(p => `<option value="${p}">${p}</option>`).join("");
    selPub.value = actual;
}
</script>

</body>
</html>
