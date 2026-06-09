<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Core Bancario - Gestión de Clientes y Asistencias</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    <script src="https://rawgit.com/schmich/instascan-builds/master/instascan.min.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        .custom-scrollbar::-webkit-scrollbar { width: 6px; height: 6px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans antialiased">

    <header class="bg-slate-900 text-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i data-lucide="landmark" class="text-emerald-400 w-8 h-8"></i>
                <span class="text-xl font-bold tracking-wider">FINANZA_CORE</span>
            </div>
            <nav class="flex space-x-1">
                <button onclick="switchTab('clientes')" id="btn-tab-clientes" class="px-4 py-2 rounded-lg text-sm font-medium bg-slate-800 text-emerald-400 flex items-center gap-2 transition">
                    <i data-lucide="users" class="w-4 h-4"></i> Clientes/Productos
                </button>
                <button onclick="switchTab('asistencias')" id="btn-tab-asistencias" class="px-4 py-2 rounded-lg text-sm font-medium text-slate-400 hover:text-white flex items-center gap-2 transition">
                    <i data-lucide="qr-code" class="w-4 h-4"></i> Control QR y Calendario
                </button>
            </nav>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-6">
        
        <section id="tab-clientes" class="space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-white p-4 rounded-xl shadow-xs gap-4">
                <div>
                    <h1 class="text-2xl font-bold text-slate-900">Portafolio de Clientes Financieros</h1>
                    <p class="text-sm text-slate-500">Administración de cuentas, saldos y estados de productos.</p>
                </div>
                <div class="flex flex-wrap gap-2 w-full sm:w-auto">
                    <button onclick="abrirModalCliente()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-4 py-2 rounded-lg text-sm font-semibold flex items-center gap-2 cursor-pointer shadow-sm transition w-full sm:w-auto justify-center">
                        <i data-lucide="user-plus" class="w-4 h-4"></i> Registrar Cliente
                    </button>
                    <button onclick="exportarBackup()" class="bg-slate-700 hover:bg-slate-800 text-white px-4 py-2 rounded-lg text-sm font-semibold flex items-center gap-2 cursor-pointer shadow-sm transition w-full sm:w-auto justify-center">
                        <i data-lucide="download" class="w-4 h-4"></i> Exportar Backup (.js)
                    </button>
                </div>
            </div>

            <div class="bg-white rounded-xl shadow-xs overflow-hidden border border-slate-100">
                <div class="overflow-x-auto custom-scrollbar">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-900 text-slate-200 text-xs font-semibold uppercase tracking-wider">
                                <th class="p-4">DNI / ID</th>
                                <th class="p-4">Cliente</th>
                                <th class="p-4">Edad</th>
                                <th class="p-4">Género</th>
                                <th class="p-4">Teléfono</th>
                                <th class="p-4">Producto Financiero</th>
                                <th class="p-4 text-center">Acciones</th>
                            </tr>
                        </thead>
                        <tbody id="tabla-clientes-body" class="divide-y divide-slate-100 text-sm">
                            </tbody>
                    </table>
                </div>
            </div>
        </section>

        <section id="tab-asistencias" class="hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <div class="bg-white p-5 rounded-xl shadow-xs border border-slate-100 flex flex-col items-center">
                    <div class="w-full border-b border-slate-100 pb-3 mb-4">
                        <h2 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                            <i data-lucide="camera" class="text-emerald-500"></i> Lector QR de Asistencia
                        </h2>
                        <p class="text-xs text-slate-500">Escanea el DNI del cliente en código QR para registrar su cita.</p>
                    </div>
                    
                    <div class="relative w-full aspect-video bg-black rounded-lg overflow-hidden flex items-center justify-center border-2 border-slate-200 shadow-inner">
                        <video id="preview" class="w-full h-full object-cover"></video>
                        <div class="absolute inset-0 border-2 border-emerald-400 opacity-40 pointer-events-none m-8 animate-pulse"></div>
                    </div>

                    <div class="mt-4 w-full flex flex-col gap-2">
                        <button onclick="iniciarEscaneoQR()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white py-2 rounded-lg text-sm font-semibold transition cursor-pointer flex justify-center items-center gap-2">
                            <i data-lucide="play" class="w-4 h-4"></i> Activar Cámara
                        </button>
                        <button onclick="registrarAsistenciaManual()" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-700 py-2 rounded-lg text-sm font-medium transition cursor-pointer text-center">
                            Registro Manual (Sin Cámara)
                        </button>
                    </div>
                </div>

                <div class="lg:col-span-2 bg-white p-5 rounded-xl shadow-xs border border-slate-100">
                    <div class="border-b border-slate-100 pb-3 mb-4 flex justify-between items-center">
                        <div>
                            <h2 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                                <i data-lucide="calendar" class="text-indigo-500"></i> Registro de Asistencias Diario
                            </h2>
                            <p class="text-xs text-slate-500">Control de citas validadas por QR en la fecha de hoy.</p>
                        </div>
                        <span id="fecha-actual" class="bg-indigo-50 text-indigo-700 text-xs px-3 py-1.5 rounded-full font-semibold"></span>
                    </div>

                    <div class="overflow-x-auto custom-scrollbar max-h-96">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-50 text-slate-500 text-xs font-semibold uppercase border-b border-slate-100">
                                    <th class="p-3">Hora</th>
                                    <th class="p-3">DNI</th>
                                    <th class="p-3">Cliente</th>
                                    <th class="p-3">Producto Asociado</th>
                                    <th class="p-3 text-center">Acción</th>
                                </tr>
                            </thead>
                            <tbody id="tabla-asistencias-body" class="divide-y divide-slate-100 text-xs">
                                </tbody>
                        </table>
                    </div>
                </div>

            </div>
        </section>

    </main>

    <script>
        // 1. BASE DE DATOS INICIAL EXIGIDA (20 REGISTROS PRE-GRABADOS)
        const registrosIniciales = [
            { dni: "47586932", nombres: "Juan Carlos", apellidos: "Mendoza Ramos", edad: 34, genero: "Masculino", telefono: "987654321", producto: "Cuenta de Ahorro Premium" },
            { dni: "71829345", nombres: "María Elena", apellidos: "Gonzáles Flores", edad: 28, genero: "Femenino", telefono: "912345678", producto: "Crédito Vehicular Interbancario" },
            { dni: "10293847", nombres: "Luis Alberto", apellidos: "Pérez Rodríguez", edad: 45, genero: "Masculino", telefono: "934567812", producto: "Tarjeta de Crédito Visa Gold" },
            { dni: "56473829", nombres: "Ana Lucía", apellidos: "Sánchez Torres", edad: 22, genero: "Femenino", telefono: "956712348", producto: "Cuenta Sueldo Efectiva" },
            { dni: "83920174", nombres: "Carlos Andrés", apellidos: "Vargas López", edad: 31, genero: "Masculino", telefono: "998123456", producto: "Crédito Hipotecario MiVivienda" },
            { dni: "29384756", nombres: "Sofia Valentina", apellidos: "Castro Morales", edad: 26, genero: "Femenino", telefono: "976543210", producto: "Fondo Mutuo Crecimiento" },
            { dni: "48572910", nombres: "Diego Armando", apellidos: "Martínez Ríos", edad: 52, genero: "Masculino", telefono: "945123789", producto: "Depósito a Plazo Fijo Extra" },
            { dni: "61029384", nombres: "Gabriela Inés", apellidos: "Gómez Díaz", edad: 39, genero: "Femenino", telefono: "921837465", producto: "Tarjeta de Crédito Platinum" },
            { dni: "38475629", nombres: "Santiago José", apellidos: "Flores Beltrán", edad: 41, genero: "Masculino", telefono: "963852741", producto: "Crédito de Capital de Trabajo" },
            { dni: "92018374", nombres: "Camila Fernanda", apellidos: "Díaz Hurtado", edad: 24, genero: "Femenino", telefono: "951753462", producto: "Cuenta de Ahorro Joven" },
            { dni: "50493827", nombres: "Jorge Enrique", apellidos: "Ramírez Vega", edad: 48, genero: "Masculino", telefono: "984736251", producto: "Seguro de Vida Inversión" },
            { dni: "18273645", nombres: "Valeria Alejandra", apellidos: "Ruiz Palacios", edad: 30, genero: "Femenino", telefono: "936251478", producto: "Tarjeta Visa Infinite" },
            { dni: "73645281", nombres: "Ricardo Manuel", apellidos: "Quispe Mamani", edad: 37, genero: "Masculino", telefono: "974125863", producto: "Crédito Mivivienda Verde" },
            { dni: "84736251", nombres: "Claudia Marcela", apellidos: "Benítez Rojas", edad: 33, genero: "Femenino", telefono: "915926483", producto: "Fondo Mutuo Conservador" },
            { dni: "26354178", nombres: "Pedro Pablo", apellidos: "Espinoza León", edad: 56, genero: "Masculino", telefono: "962481357", producto: "Depósito a Plazo Fijo Oro" },
            { dni: "95184627", nombres: "Laura Cristina", apellidos: "Villanueva Cruz", edad: 29, genero: "Femenino", telefono: "983174625", producto: "Cuenta Sueldo Preferencial" },
            { dni: "35795146", nombres: "Mateo Ignacio", apellidos: "Gutiérrez Solís", edad: 27, genero: "Masculino", telefono: "971364258", producto: "Crédito Vehicular Eco" },
            { dni: "62481539", nombres: "Andrea Beatriz", apellidos: "Navarro Peña", edad: 42, genero: "Femenino", telefono: "924681357", producto: "Tarjeta Mastercard Black" },
            { dni: "46825139", nombres: "Miguel Ángel", apellidos: "Campos Ordóñez", edad: 50, genero: "Masculino", telefono: "953147862", producto: "Crédito Comercial Empresa" },
            { dni: "75315946", nombres: "Natalia Isabel", apellidos: "Mendoza Solano", edad: 35, genero: "Femenino", telefono: "996358214", producto: "Cuenta de Ahorro Emprendedor" }
        ];

        // Inicialización de LocalStorage si está vacío
        if (!localStorage.getItem('clientes_financieros')) {
            localStorage.setItem('clientes_financieros', JSON.stringify(registrosIniciales));
        }
        if (!localStorage.getItem('asistencias_financieras')) {
            localStorage.setItem('asistencias_financieras', JSON.stringify([]));
        }

        let clientes = JSON.parse(localStorage.getItem('clientes_financieros'));
        let asistencias = JSON.parse(localStorage.getItem('asistencias_financieras'));
        let scanner = null;

        // Mostrar Fecha de Hoy
        document.getElementById('fecha-actual').innerText = new Date().toLocaleDateString('es-ES', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });

        // 2. CONTROLADORES DE PESTAÑAS (TABS)
        function switchTab(tab) {
            document.getElementById('tab-clientes').classList.add('hidden');
            document.getElementById('tab-asistencias').classList.add('hidden');
            document.getElementById('btn-tab-clientes').className = "px-4 py-2 rounded-lg text-sm font-medium text-slate-400 hover:text-white flex items-center gap-2 transition";
            document.getElementById('btn-tab-asistencias').className = "px-4 py-2 rounded-lg text-sm font-medium text-slate-400 hover:text-white flex items-center gap-2 transition";

            if(tab === 'clientes') {
                document.getElementById('tab-clientes').classList.remove('hidden');
                document.getElementById('btn-tab-clientes').className = "px-4 py-2 rounded-lg text-sm font-medium bg-slate-800 text-emerald-400 flex items-center gap-2 transition";
                if(scanner) { scanner.stop(); }
            } else {
                document.getElementById('tab-asistencias').classList.remove('hidden');
                document.getElementById('btn-tab-asistencias').className = "px-4 py-2 rounded-lg text-sm font-medium bg-slate-800 text-emerald-400 flex items-center gap-2 transition";
            }
        }

        // 3. RENDERIZADO DE TABLAS
        function renderTablaClientes() {
            const tbody = document.getElementById('tabla-clientes-body');
            tbody.innerHTML = '';
            clientes.forEach((c, index) => {
                tbody.innerHTML += `
                    <tr class="hover:bg-slate-50/80 transition">
                        <td class="p-4 font-mono font-semibold text-slate-600">${c.dni}</td>
                        <td class="p-4 font-semibold text-slate-900">${c.nombres} ${c.apellidos}</td>
                        <td class="p-4 text-slate-600">${c.edad} años</td>
                        <td class="p-4">
                            <span class="px-2 py-1 rounded-full text-xs font-medium ${c.genero === 'Masculino' ? 'bg-blue-50 text-blue-700':'bg-pink-50 text-pink-700'}">${c.genero}</span>
                        </td>
                        <td class="p-4 text-slate-600">${c.telefono}</td>
                        <td class="p-4 font-medium text-slate-700"><span class="bg-slate-100 px-2 py-1 rounded text-xs border border-slate-200">${c.producto}</span></td>
                        <td class="p-4 text-center flex justify-center space-x-2">
                            <button onclick="abrirModalCliente(${index})" class="text-blue-600 hover:text-blue-800 p-1 rounded hover:bg-blue-50 transition cursor-pointer" title="Editar"><i data-lucide="edit-3" class="w-4 h-4"></i></button>
                            <button onclick="eliminarCliente(${index})" class="text-rose-600 hover:text-rose-800 p-1 rounded hover:bg-rose-50 transition cursor-pointer" title="Eliminar"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                        </td>
                    </tr>
                `;
            });
            lucide.createIcons();
        }

        function renderTablaAsistencias() {
            const tbody = document.getElementById('tabla-asistencias-body');
            tbody.innerHTML = '';
            if(asistencias.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="p-4 text-center text-slate-400">Ningún cliente ha registrado asistencia mediante QR hoy.</td></tr>`;
                return;
            }
            asistencias.forEach((a, index) => {
                tbody.innerHTML += `
                    <tr class="hover:bg-slate-50">
                        <td class="p-3 font-mono text-indigo-600 font-semibold">${a.hora}</td>
                        <td class="p-3 font-mono">${a.dni}</td>
                        <td class="p-3 font-semibold text-slate-800">${a.nombre}</td>
                        <td class="p-3 text-slate-600">${a.producto}</td>
                        <td class="p-3 text-center">
                            <button onclick="eliminarAsistencia(${index})" class="text-rose-500 hover:text-rose-700 p-1 cursor-pointer"><i data-lucide="x-circle" class="w-4 h-4"></i></button>
                        </td>
                    </tr>
                `;
            });
            lucide.createIcons();
        }

        // 4. OPERACIONES CRUD CON SWEETALERT2
        async function abrirModalCliente(index = null) {
            const esEditar = index !== null;
            const cliente = esEditar ? clientes[index] : { dni: '', nombres: '', apellidos: '', edad: '', genero: 'Masculino', telefono: '', producto: '' };

            const { value: formValues } = await Swal.fire({
                title: esEditar ? '<span class="text-xl font-bold">Editar Cliente</span>' : '<span class="text-xl font-bold">Nuevo Registro Financiero</span>',
                html: `
                    <div class="text-left space-y-3 text-sm p-1">
                        <div><label class="block text-xs font-semibold text-slate-500 mb-1">DNI (8 dígitos)</label><input id="swal-dni" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value="${cliente.dni}" ${esEditar ? 'disabled':''}></div>
                        <div><label class="block text-xs font-semibold text-slate-500 mb-1">Nombres</label><input id="swal-nombres" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value="${cliente.nombres}"></div>
                        <div><label class="block text-xs font-semibold text-slate-500 mb-1">Apellidos</label><input id="swal-apellidos" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value="${cliente.apellidos}"></div>
                        <div class="grid grid-cols-2 gap-2">
                            <div><label class="block text-xs font-semibold text-slate-500 mb-1">Edad</label><input id="swal-edad" type="number" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value="${cliente.edad}"></div>
                            <div><label class="block text-xs font-semibold text-slate-500 mb-1">Género</label><select id="swal-genero" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none"><option value="Masculino" ${cliente.genero==='Masculino'?'selected':''}>Masculino</option><option value="Femenino" ${cliente.genero==='Femenino'?'selected':''}>Femenino</option></select></div>
                        </div>
                        <div><label class="block text-xs font-semibold text-slate-500 mb-1">Teléfono móvil</label><input id="swal-telefono" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value="${cliente.telefono}"></div>
                        <div><label class="block text-xs font-semibold text-slate-500 mb-1">Producto Financiero Asignado</label><input id="swal-producto" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-emerald-500 outline-none" value="${cliente.producto}" placeholder="Ej. Tarjeta Gold, Crédito Pyme"></div>
                    </div>
                `,
                focusConfirm: false,
                confirmButtonText: esEditar ? 'Actualizar':'Guardar',
                confirmButtonColor: '#059669',
                showCancelButton: true,
                cancelButtonText: 'Cancelar',
                preConfirm: () => {
                    const dni = document.getElementById('swal-dni').value.trim();
                    const nombres = document.getElementById('swal-nombres').value.trim();
                    const apellidos = document.getElementById('swal-apellidos').value.trim();
                    const edad = document.getElementById('swal-edad').value;
                    const genero = document.getElementById('swal-genero').value;
                    const telefono = document.getElementById('swal-telefono').value.trim();
                    const producto = document.getElementById('swal-producto').value.trim();

                    if (!dni || !nombres || !apellidos || !edad || !telefono || !producto) {
                        Swal.showValidationMessage('Todos los campos son obligatorios');
                        return false;
                    }
                    if (dni.length !== 8 || isNaN(dni)) {
                        Swal.showValidationMessage('El DNI debe tener exactamente 8 caracteres numéricos');
                        return false;
                    }
                    if (!esEditar && clientes.some(c => c.dni === dni)) {
                        Swal.showValidationMessage('Este número de DNI ya se encuentra registrado');
                        return false;
                    }

                    return { dni, nombres, apellidos, edad: parseInt(edad), genero, telefono, producto };
                }
            });

            if (formValues) {
                if (esEditar) {
                    clientes[index] = formValues;
                    Swal.fire({ icon: 'success', title: 'Modificado con éxito', showConfirmButton: false, timer: 1500 });
                } else {
                    clientes.unshift(formValues);
                    Swal.fire({ icon: 'success', title: 'Cliente registrado', showConfirmButton: false, timer: 1500 });
                }
                actualizarLocalStorage();
            }
        }

        function eliminarCliente(index) {
            Swal.fire({
                title: '¿Estás completamente seguro?',
                text: `Se eliminará el perfil del cliente e interrumpirá sus productos asociados.`,
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#e11d48',
                cancelButtonColor: '#64748b',
                confirmButtonText: 'Sí, dar de baja',
                cancelButtonText: 'Cancelar'
            }).then((result) => {
                if (result.isConfirmed) {
                    clientes.splice(index, 1);
                    actualizarLocalStorage();
                    Swal.fire({ icon: 'success', title: 'Eliminado', text: 'El registro ha sido removido del sistema.', showConfirmButton: false, timer: 1500 });
                }
            });
        }

        // 5. CÁMARA E INTEGRACIÓN DE LECTOR QR (Instascan)
        function iniciarEscaneoQR() {
            if(scanner) {
                scanner.stop();
            }
            
            scanner = new Instascan.Scanner({ video: document.getElementById('preview'), mirror: false });
            
            scanner.addListener('scan', function (content) {
                procesarIngresoAsistencia(content.trim());
            });

            Instascan.Camera.getCameras().then(function (cameras) {
                if (cameras.length > 0) {
                    // Selecciona de forma preferente la cámara trasera en dispositivos móviles
                    let selectedCamera = cameras[0];
                    for (let i = 0; i < cameras.length; i++) {
                        if (cameras[i].name.toLowerCase().includes('back') || cameras[i].name.toLowerCase().includes('trasera')) {
                            selectedCamera = cameras[i];
                            break;
                        }
                    }
                    scanner.start(selectedCamera).catch(e => {
                        Swal.fire('Error de Acceso', 'No se pudo abrir el hardware de la cámara.', 'error');
                    });
                } else {
                    Swal.fire('Hardware Ausente', 'No se detectaron cámaras en este dispositivo.', 'warning');
                }
            }).catch(function (e) {
                console.error(e);
                Swal.fire('Error', 'Error de permisos al invocar la cámara de video.', 'error');
            });
        }

        function procesarIngresoAsistencia(dniBuscado) {
            const clienteEncontrado = clientes.find(c => c.dni === dniBuscado);

            if (!clienteEncontrado) {
                Swal.fire({ icon: 'error', title: 'No Encontrado', text: `El código QR escaneado (DNI: ${dniBuscado}) no corresponde a ningún cliente vigente.` });
                return;
            }

            const yaIngreso = asistencias.some(a => a.dni === dniBuscado);
            if (yaIngreso) {
                Swal.fire({ icon: 'info', title: 'Asistencia Duplicada', text: `${clienteEncontrado.nombres} ya registró su ingreso el día de hoy.` });
                return;
            }

            const ahora = new Date();
            const horaString = ahora.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit', second: '2-digit' });

            const nuevaAsistencia = {
                hora: horaString,
                dni: clienteEncontrado.dni,
                nombre: `${clienteEncontrado.nombres} ${clienteEncontrado.apellidos}`,
                producto: clienteEncontrado.producto
            };

            asistencias.unshift(nuevaAsistencia);
            localStorage.setItem('asistencias_financieras', JSON.stringify(asistencias));
            renderTablaAsistencias();

            Swal.fire({
                icon: 'success',
                title: 'Asistencia Confirmada',
                html: `<b>Cliente:</b> ${nuevaAsistencia.nombre}<br><b>Producto:</b> ${nuevaAsistencia.producto}<br><b>Hora:</b> ${horaString}`,
                timer: 2500
            });
        }

        async function registrarAsistenciaManual() {
            const { value: dni } = await Swal.fire({
                title: 'Registro de Asistencia Manual',
                input: 'text',
                inputLabel: 'Ingrese el número de DNI del cliente:',
                inputPlaceholder: 'Ej. 47586932',
                showCancelButton: true,
                confirmButtonColor: '#4f46e5'
            });
            if(dni) {
                procesarIngresoAsistencia(dni.trim());
            }
        }

        function eliminarAsistencia(index) {
            asistencias.splice(index, 1);
            localStorage.setItem('asistencias_financieras', JSON.stringify(asistencias));
            renderTablaAsistencias();
        }

        // 6. COPIA DE SEGURIDAD (BACKUP DE ARCHIVO .JS)
        function exportarBackup() {
            const dataToExport = {
                fecha_respaldo: new Date().toISOString(),
                total_clientes: clientes.length,
                clientes: clientes,
                asistencias_hoy: asistencias
            };

            // Convertimos la información en código estructurado Javascript ejecutable o almacenable
            const scriptContenido = `/** BACKUP CORE FINANCIERO - GENERADO AUTOMÁTICAMENTE **/\nconst BACKUP_DATA = ${JSON.stringify(dataToExport, null, 4)};`;
            
            const blob = new Blob([scriptContenido], { type: 'application/javascript;charset=utf-8;' });
            const link = document.createElement("a");
            
            const url = URL.createObjectURL(blob);
            link.setAttribute("href", url);
            link.setAttribute("download", `backup_financiero_${new Date().toISOString().slice(0,10)}.js`);
            link.style.visibility = 'hidden';
            
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);

            Swal.fire('Respaldo Exitoso', 'Se ha descargado un script de Javascript con los 20 registros iniciales y las modificaciones vigentes.', 'success');
        }

        function actualizarLocalStorage() {
            localStorage.setItem('clientes_financieros', JSON.stringify(clientes));
            renderTablaClientes();
        }

        // Arranque de interfaz inicial
        renderTablaClientes();
        renderTablaAsistencias();
    </script>
</body>
</html>
