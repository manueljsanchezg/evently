<template>
  <div class="min-h-screen flex flex-col">
    <header class="sticky top-0 z-40 bg-navy text-cream shadow-md">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
        <button type="button" class="flex items-center gap-3" @click="switchTab('explorar')">
          <div class="w-10 h-10 rounded-full bg-cream text-navy flex items-center justify-center font-serif font-bold text-2xl relative">
            e
            <span class="text-gold text-xs absolute -top-0.5 -right-0.5">✦</span>
          </div>
          <div class="text-left">
            <span class="font-serif text-2xl font-bold tracking-tight">evently</span>
            <p class="text-[10px] tracking-widest uppercase text-gold -mt-1">Conecta · Reserva · Celebra</p>
          </div>
        </button>

        <nav class="hidden md:flex items-center gap-8 text-sm font-medium">
          <button
            type="button"
            class="hover:text-gold transition"
            :class="activeTab === 'explorar' ? 'text-gold underline underline-offset-8' : ''"
            @click="switchTab('explorar')"
          >
            Explorar Proveedores
          </button>
          <button type="button" class="hover:text-gold transition flex items-center gap-1.5" :class="activeTab === 'comparador' ? 'text-gold underline underline-offset-8' : ''" @click="switchTab('comparador')">
            Comparador
            <span class="bg-gold text-navy text-xs font-bold px-2 py-0.5 rounded-full">{{ compareList.length }}</span>
          </button>
          <button type="button" class="hover:text-gold transition flex items-center gap-1.5" :class="activeTab === 'mievento' ? 'text-gold underline underline-offset-8' : ''" @click="switchTab('mievento')">
            Mi Evento
            <span class="bg-cream text-navy text-xs font-bold px-2 py-0.5 rounded-full">{{ selectedServices.length }}</span>
          </button>
        </nav>

        <button type="button" class="bg-gold hover:bg-goldDark text-navy font-bold text-sm px-5 py-2.5 rounded-full transition shadow-sm" @click="openPreventaModal">
          Acceso Preventa ✨
        </button>
      </div>

      <div class="md:hidden flex justify-around bg-navyLight py-2.5 text-xs border-t border-white/10">
        <button type="button" class="text-cream font-semibold" @click="switchTab('explorar')">Explorar</button>
        <button type="button" class="text-cream font-semibold" @click="switchTab('comparador')">Comparador ({{ compareList.length }})</button>
        <button type="button" class="text-cream font-semibold" @click="switchTab('mievento')">Mi Evento ({{ selectedServices.length }})</button>
      </div>
    </header>

    <main v-if="activeTab === 'explorar'" class="flex-1">
      <section class="bg-navy text-cream pt-12 pb-20 px-4 relative overflow-hidden">
        <div class="max-w-5xl mx-auto text-center relative z-10">
          <span class="inline-block bg-white/10 text-gold text-xs font-semibold tracking-wider uppercase px-4 py-1.5 rounded-full mb-5 border border-gold/30">
            Fase Preventa · Precios 100 % transparentes
          </span>
          <h1 class="font-serif text-4xl sm:text-5xl lg:text-6xl font-bold leading-tight mb-6">
            Organiza tu celebración con <span class="text-gold italic">precios reales</span> desde el segundo uno
          </h1>
          <p class="text-cream/85 text-base sm:text-lg max-w-2xl mx-auto mb-10">
            Olvídate de mandar 20 mensajes esperando presupuesto. Compara catering, fotografía, decoración y música con tarifas cerradas y reserva en minutos.
          </p>

          <div class="bg-creamSoft text-navy p-4 sm:p-5 rounded-2xl shadow-xl max-w-4xl mx-auto grid grid-cols-1 sm:grid-cols-4 gap-3 text-left">
            <div>
              <label class="block text-[11px] font-bold uppercase tracking-wider text-navy/60 mb-1">Categoría</label>
              <select v-model="filterCategory" class="w-full bg-cream border border-navy/15 rounded-xl px-3 py-2.5 text-sm font-medium focus:outline-none focus:border-navy">
                <option value="Todas">Todas las categorías</option>
                <option value="Catering">Catering y Cóctel</option>
                <option value="Fotografía">Fotografía y Vídeo</option>
                <option value="Floristería">Floristería y Deco</option>
                <option value="Música">Música y DJ</option>
                <option value="Espacios">Haciendas y Espacios</option>
              </select>
            </div>
            <div>
              <label class="block text-[11px] font-bold uppercase tracking-wider text-navy/60 mb-1">Ciudad</label>
              <select v-model="filterCity" class="w-full bg-cream border border-navy/15 rounded-xl px-3 py-2.5 text-sm font-medium focus:outline-none focus:border-navy">
                  <option value="Todas">Toda Sevilla y provincia</option>
                <option value="Sevilla">Sevilla</option>
                  <option value="Salteras">Salteras</option>
                  <option value="Utrera">Utrera</option>
                  <option value="Carmona">Carmona</option>
                  <option value="Espartinas">Espartinas</option>
              </select>
            </div>
            <div>
              <label class="block text-[11px] font-bold uppercase tracking-wider text-navy/60 mb-1">Tipo de evento</label>
              <select v-model="filterEvent" class="w-full bg-cream border border-navy/15 rounded-xl px-3 py-2.5 text-sm font-medium focus:outline-none focus:border-navy">
                <option value="Todos">Cualquier evento</option>
                <option value="Navidad">Cena de Navidad / Casa</option>
                <option value="Cumpleaños">Cumpleaños / Fiesta</option>
                <option value="Boda">Boda / Comunión</option>
                <option value="Graduación">Graduación / Empresa</option>
              </select>
            </div>
            <div class="flex items-end">
              <button type="button" class="w-full bg-navy hover:bg-navyLight text-cream font-semibold py-2.5 px-4 rounded-xl text-sm transition" @click="resetFilters">
                Mostrar todos
              </button>
            </div>
          </div>
        </div>
      </section>

      <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-14">
        <div class="flex flex-col sm:flex-row sm:items-end justify-between mb-8 gap-4">
          <div>
            <h2 class="font-serif text-3xl font-bold text-navy">Proveedores verificados con tarifa visible</h2>
            <p class="text-navy/70 text-sm mt-1">Haz clic en «Ver desglose» para calcular el precio exacto según tus invitados.</p>
          </div>
          <span class="text-xs font-bold uppercase tracking-wider bg-navy/10 text-navy px-3.5 py-1.5 rounded-full self-start">
            {{ filteredProviders.length }} {{ filteredProviders.length === 1 ? 'proveedor disponible' : 'proveedores disponibles' }}
          </span>
        </div>

        <div v-if="filteredProviders.length" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-7">
          <article v-for="provider in filteredProviders" :key="provider.id" class="bg-creamSoft rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition gold-border flex flex-col">
            <div class="relative h-52">
              <img :src="provider.image" :alt="provider.name" class="w-full h-full object-cover">
              <span class="absolute top-3 left-3 bg-navy/90 text-cream text-xs font-semibold px-3 py-1 rounded-full">
                {{ provider.category }} · {{ provider.city }}
              </span>
              <span class="absolute bottom-3 right-3 bg-cream text-navy text-xs font-bold px-3 py-1 rounded-full shadow">
                ★ {{ provider.rating }} ({{ provider.reviews }})
              </span>
            </div>
            <div class="p-6 flex-1 flex flex-col justify-between">
              <div>
                <h3 class="font-serif text-xl font-bold text-navy leading-snug mb-2">{{ provider.name }}</h3>
                <p class="text-xs text-navy/75 leading-relaxed mb-4">{{ provider.summary }}</p>
              </div>
              <div>
                <div class="bg-cream rounded-xl p-3 mb-4 flex items-center justify-between border border-gold/30">
                  <div>
                    <span class="block text-[10px] uppercase tracking-wider font-bold text-navy/60">Tarifa transparente</span>
                    <span class="font-serif text-xl font-bold text-navy">{{ provider.basePrice }} € <span class="text-xs font-sans font-normal text-navy/70">{{ provider.pricingType === 'per_person' ? '/ persona' : 'cerrado' }}</span></span>
                  </div>
                  <span class="text-[11px] font-bold text-goldDark bg-gold/15 px-2.5 py-1 rounded-full">Precio real</span>
                </div>
                <div class="grid grid-cols-2 gap-2">
                  <button type="button" class="bg-navy hover:bg-navyLight text-cream text-xs font-bold py-2.5 px-3 rounded-xl transition" @click="openDetailModal(provider)">
                    Ver desglose
                  </button>
                  <button type="button" class="text-xs font-bold py-2.5 px-3 rounded-xl transition" :class="compareList.includes(provider.id) ? 'bg-gold text-navy' : 'bg-cream border border-navy/20 text-navy hover:bg-navy/5'" @click="toggleCompare(provider.id)">
                    {{ compareList.includes(provider.id) ? '✓ En comparador' : '+ Comparar' }}
                  </button>
                </div>
              </div>
            </div>
          </article>
        </div>

        <div v-else class="col-span-full text-center py-12 bg-creamSoft rounded-2xl gold-border">
          <p class="font-serif text-xl font-bold mb-2">No hay proveedores con esa combinación exacta</p>
          <button type="button" class="mt-2 bg-navy text-cream text-xs font-bold px-4 py-2 rounded-full" @click="resetFilters">Ver todos los proveedores</button>
        </div>
      </section>

      <section class="bg-navy text-cream py-16 px-4 mt-8">
        <div class="max-w-6xl mx-auto">
          <div class="text-center mb-12">
            <span class="text-gold text-xs font-bold uppercase tracking-widest">Sin llamadas ni esperas</span>
            <h2 class="font-serif text-3xl sm:text-4xl font-bold mt-2">Cómo funciona Evently</h2>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div v-for="(step, index) in howItWorks" :key="step.title" class="bg-navyLight p-7 rounded-2xl border border-gold/25">
              <div class="w-10 h-10 rounded-full bg-gold text-navy font-bold flex items-center justify-center mb-4">{{ index + 1 }}</div>
              <h3 class="font-serif text-xl font-bold mb-2">{{ step.title }}</h3>
              <p class="text-sm text-cream/80 leading-relaxed">{{ step.text }}</p>
            </div>
          </div>
        </div>
      </section>
    </main>

    <section v-if="activeTab === 'comparador'" class="flex-1 max-w-7xl mx-auto w-full px-4 sm:px-6 lg:px-8 py-12">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-8">
        <div>
          <span class="text-xs font-bold uppercase tracking-wider text-goldDark">Comparativa directa</span>
          <h2 class="font-serif text-3xl font-bold text-navy">Compara tus proveedores favoritos</h2>
          <p class="text-sm text-navy/70">Selecciona hasta 3 proveedores desde el catálogo para compararlos frente a frente.</p>
        </div>
        <button type="button" class="bg-navy text-cream text-sm font-semibold px-5 py-2.5 rounded-full self-start" @click="switchTab('explorar')">
          + Añadir proveedores
        </button>
      </div>

      <div class="bg-creamSoft rounded-2xl shadow-md gold-border p-6 overflow-x-auto">
        <div v-if="comparedProviders.length === 0" class="text-center py-12">
          <p class="font-serif text-xl font-bold mb-2">Aún no has seleccionado proveedores para comparar</p>
          <p class="text-sm text-navy/70 mb-4">Pulsa «+ Comparar» en las tarjetas del catálogo para ver sus tarifas y condiciones lado a lado.</p>
          <button type="button" class="bg-navy text-cream text-xs font-bold px-5 py-2.5 rounded-full" @click="switchTab('explorar')">Ir al catálogo</button>
        </div>
        <table v-else class="w-full text-left border-collapse min-w-[600px]">
          <thead>
            <tr class="border-b border-navy/15">
              <th class="py-4 px-3 text-xs uppercase tracking-wider text-navy/60 w-1/4">Característica</th>
              <th v-for="provider in comparedProviders" :key="provider.id" class="py-4 px-3 align-top">
                <div class="font-serif text-lg font-bold text-navy">{{ provider.name }}</div>
                <span class="text-xs text-goldDark font-semibold">{{ provider.category }} · {{ provider.city }}</span>
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-navy/10 text-sm">
            <tr>
              <td class="py-4 px-3 font-semibold">Tarifa transparente</td>
              <td v-for="provider in comparedProviders" :key="provider.id" class="py-4 px-3 font-serif text-xl font-bold text-navy">{{ provider.basePrice }} € <span class="text-xs font-sans font-normal">{{ provider.unitLabel }}</span></td>
            </tr>
            <tr>
              <td class="py-4 px-3 font-semibold">Valoración</td>
              <td v-for="provider in comparedProviders" :key="provider.id" class="py-4 px-3">★ <strong>{{ provider.rating }}</strong> ({{ provider.reviews }} reseñas)</td>
            </tr>
            <tr>
              <td class="py-4 px-3 font-semibold align-top">Qué incluye</td>
              <td v-for="provider in comparedProviders" :key="provider.id" class="py-4 px-3 align-top">
                <ul class="space-y-1 text-xs"><li v-for="item in provider.includes" :key="item">✓ {{ item }}</li></ul>
              </td>
            </tr>
            <tr>
              <td class="py-4 px-3 font-semibold">Cancelación</td>
              <td v-for="provider in comparedProviders" :key="provider.id" class="py-4 px-3 text-xs">{{ provider.cancellation }}</td>
            </tr>
            <tr>
              <td class="py-4 px-3"></td>
              <td v-for="provider in comparedProviders" :key="provider.id" class="py-4 px-3 space-y-2">
                <button type="button" class="block w-full bg-navy text-cream text-xs font-bold py-2 px-3 rounded-lg" @click="openDetailModal(provider)">Elegir y calcular</button>
                <button type="button" class="block w-full text-xs text-navy/60 hover:text-navy underline" @click="toggleCompare(provider.id)">Quitar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section v-if="activeTab === 'mievento'" class="flex-1 max-w-7xl mx-auto w-full px-4 sm:px-6 lg:px-8 py-12">
      <div class="mb-8">
        <span class="text-xs font-bold uppercase tracking-wider text-goldDark">Panel de organización</span>
        <h2 class="font-serif text-3xl font-bold text-navy">Mi Evento y Presupuesto Cerrado</h2>
        <p class="text-sm text-navy/70">Controla el coste exacto de tu celebración sin sorpresas finales y gestiona tus tareas.</p>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
        <div class="lg:col-span-2 space-y-4">
          <div class="bg-creamSoft rounded-2xl p-6 shadow-sm gold-border">
            <div class="flex items-center justify-between mb-4">
              <h3 class="font-serif text-xl font-bold">Servicios seleccionados</h3>
              <button type="button" class="text-xs font-bold text-navy underline" @click="switchTab('explorar')">+ Explorar más</button>
            </div>
            <div class="space-y-3">
              <p v-if="selectedServices.length === 0" class="text-sm text-navy/60 py-6 text-center bg-cream rounded-xl">Todavía no has añadido servicios. Explora el catálogo y calcula tu presupuesto.</p>
              <div v-for="service in selectedServices" :key="service.uid" class="flex items-center justify-between bg-cream p-4 rounded-xl border border-navy/10">
                <div>
                  <span class="text-[11px] font-bold uppercase text-goldDark">{{ service.category }} · {{ service.detailNote }}</span>
                  <h4 class="font-bold text-navy">{{ service.name }}</h4>
                </div>
                <div class="flex items-center gap-4">
                  <span class="font-serif text-lg font-bold">{{ service.total }} €</span>
                  <button type="button" class="text-xs text-red-600 hover:underline" @click="removeService(service.uid)">Quitar</button>
                </div>
              </div>
            </div>
          </div>

          <div class="bg-creamSoft rounded-2xl p-6 shadow-sm gold-border">
            <h3 class="font-serif text-xl font-bold mb-4">Checklist de tu celebración</h3>
            <div class="flex gap-2 mb-4">
              <input v-model="newTask" type="text" placeholder="Añadir tarea (ej. Confirmar alergias del menú)..." class="flex-1 bg-cream border border-navy/20 rounded-xl px-4 py-2 text-sm focus:outline-none focus:border-navy" @keyup.enter="addTask">
              <button type="button" class="bg-navy text-cream text-sm font-semibold px-4 py-2 rounded-xl hover:bg-navyLight" @click="addTask">Añadir</button>
            </div>
            <ul class="space-y-2 text-sm">
              <li v-for="task in tasks" :key="task.id" class="flex items-center justify-between bg-cream px-4 py-2.5 rounded-xl">
                <label class="flex items-center gap-3 cursor-pointer">
                  <input v-model="task.done" type="checkbox" class="accent-navy w-4 h-4">
                  <span :class="task.done ? 'line-through text-navy/50' : 'text-navy'">{{ task.text }}</span>
                </label>
              </li>
            </ul>
          </div>
        </div>

        <div class="bg-navy text-cream rounded-2xl p-7 shadow-lg h-fit">
          <span class="text-gold text-xs font-bold uppercase tracking-wider">Resumen transparente</span>
          <h3 class="font-serif text-2xl font-bold mt-1 mb-6">Presupuesto Total</h3>
          <div class="space-y-3 text-sm border-b border-white/15 pb-5 mb-5">
            <div class="flex justify-between"><span class="text-cream/75">Servicios añadidos</span><span class="font-semibold">{{ selectedServices.length }}</span></div>
            <div class="flex justify-between"><span class="text-cream/75">Comisión de gestión Evently</span><span class="text-gold font-semibold">0 € (Gratis en Preventa)</span></div>
            <div class="flex justify-between"><span class="text-cream/75">IVA e impuestos</span><span class="font-semibold">Incluidos</span></div>
          </div>
          <div class="flex justify-between items-baseline mb-6">
            <span class="text-base font-medium">Total estimado</span>
            <span class="font-serif text-3xl font-bold text-gold">{{ eventTotal }} €</span>
          </div>
          <button type="button" class="w-full bg-gold hover:bg-goldDark text-navy font-bold py-3.5 rounded-xl transition shadow-md" @click="simulateBooking">
            Simular Reserva Directa
          </button>
          <p class="text-[11px] text-cream/60 text-center mt-3">Precio cerrado garantizado sin cargos ocultos.</p>
        </div>
      </div>
    </section>

    <footer class="bg-navy text-cream/80 py-10 px-4 border-t border-gold/20 mt-16">
      <div class="max-w-7xl mx-auto flex flex-col gap-6 text-sm">
        <div class="flex flex-col sm:flex-row items-center justify-between gap-4">
          <div class="flex items-center gap-2.5">
            <span class="font-serif text-xl font-bold text-cream">evently</span>
            <span class="text-gold">·</span>
            <span class="text-xs uppercase tracking-widest text-gold">Conecta · Reserva · Celebra</span>
          </div>
          <nav aria-label="Información legal" class="flex flex-wrap items-center justify-center gap-x-5 gap-y-2">
            <button type="button" class="hover:text-gold underline-offset-4 hover:underline focus-visible:outline focus-visible:outline-2 focus-visible:outline-gold rounded" @click="openLegalModal('aviso')">
              Aviso Legal
            </button>
            <button type="button" class="hover:text-gold underline-offset-4 hover:underline focus-visible:outline focus-visible:outline-2 focus-visible:outline-gold rounded" @click="openLegalModal('privacidad')">
              Política de Privacidad
            </button>
            <button type="button" class="hover:text-gold underline-offset-4 hover:underline focus-visible:outline focus-visible:outline-2 focus-visible:outline-gold rounded" @click="openLegalModal('cookies')">
              Política de Cookies
            </button>
            <button type="button" class="font-semibold text-gold hover:text-cream underline underline-offset-4 focus-visible:outline focus-visible:outline-2 focus-visible:outline-gold rounded" @click="openCookiePreferences">
              Personalizar Cookies
            </button>
          </nav>
        </div>
        <p class="text-xs text-cream/60 text-center sm:text-right">Proyecto académico · Prototipo funcional de campaña de preventa.</p>
      </div>
    </footer>

    <div v-if="legalModalOpen" class="fixed inset-0 z-[60] bg-navy/80 backdrop-blur-sm flex items-center justify-center p-3 sm:p-6" role="dialog" aria-modal="true" :aria-labelledby="legalModalTitleId" @click.self="closeLegalModal">
      <article class="bg-creamSoft text-navy rounded-2xl max-w-4xl w-full shadow-2xl relative max-h-[92vh] overflow-y-auto border border-gold/50">
        <div class="sticky top-0 z-10 bg-navy text-cream px-5 sm:px-8 py-5 flex items-start justify-between gap-4 border-b border-gold/30">
          <div>
            <span class="text-gold text-xs font-bold uppercase tracking-widest">Evently · Información legal</span>
            <h2 :id="legalModalTitleId" class="font-serif text-2xl sm:text-3xl font-bold mt-1">{{ legalTitle }}</h2>
          </div>
          <button type="button" class="shrink-0 w-9 h-9 rounded-full bg-white/10 hover:bg-gold hover:text-navy flex items-center justify-center font-bold focus-visible:outline focus-visible:outline-2 focus-visible:outline-gold" :aria-label="`Cerrar ${legalTitle}`" @click="closeLegalModal">✕</button>
        </div>

        <div class="px-5 sm:px-8 py-7 sm:py-9 text-[15px] leading-7 legal-content">
          <template v-if="legalSection === 'aviso'">
            <p class="mb-6">Este Aviso Legal regula el acceso y uso del sitio web <a :href="siteUrl" target="_blank" rel="noopener noreferrer" class="font-semibold underline">{{ siteUrl }}</a>, propiedad de Evently.</p>

            <section class="legal-section">
              <h3>1. Identificación y titularidad</h3>
              <dl>
                <div><dt>Nombre comercial / titular</dt><dd>Evently</dd></div>
                <div><dt>Domicilio social</dt><dd>Calle Américo Vespucio, 27, 41092 Sevilla, España</dd></div>
                <div><dt>Correo electrónico</dt><dd><a :href="`mailto:${contactEmail}`">{{ contactEmail }}</a></dd></div>
                <div><dt>Actividad</dt><dd>Plataforma marketplace digital para conectar usuarios con proveedores reales de catering y cóctel, fotografía y vídeo, floristería y decoración, y música y DJ en Sevilla y Andalucía.</dd></div>
              </dl>
              <p>Esta información se ofrece de conformidad con el artículo 10 de la Ley 34/2002, de 11 de julio, de Servicios de la Sociedad de Información y de Comercio Electrónico (LSSI-CE).</p>
            </section>

            <section class="legal-section">
              <h3>2. Objeto y condiciones de uso</h3>
              <p>El sitio facilita información sobre servicios de proveedores de eventos y permite consultar, comparar y gestionar una selección de proveedores. Evently actúa como plataforma de intermediación; las reservas, prestaciones y relaciones contractuales pueden realizarse directamente con el proveedor correspondiente.</p>
              <p>El usuario se compromete a utilizar el sitio de forma lícita, diligente y respetuosa, sin vulnerar derechos de terceros ni realizar actividades que dañen, inhabiliten o sobrecarguen los sistemas de Evently.</p>
            </section>

            <section class="legal-section">
              <h3>3. Propiedad intelectual e industrial</h3>
              <p>Los textos, diseños, marcas, logotipos, interfaces, fotografías, estructura y demás contenidos del sitio están protegidos por la legislación española e internacional sobre propiedad intelectual e industrial. El uso no autorizado de cualquiera de estos elementos puede dar lugar a las responsabilidades legalmente establecidas.</p>
            </section>

            <section class="legal-section">
              <h3>4. Responsabilidad y enlaces externos</h3>
              <p>Evently adopta medidas razonables para mantener actualizada la información, pero no garantiza su exactitud absoluta ni la disponibilidad continua del sitio. Los enlaces a proveedores, redes sociales y otros sitios externos se ofrecen a modo informativo; Evently no controla ni asume responsabilidad por sus contenidos, políticas o prácticas.</p>
            </section>

            <section class="legal-section">
              <h3>5. Legislación y jurisdicción</h3>
              <p>Las presentes condiciones se rigen por la legislación española. Para cualquier controversia derivada del uso del sitio, las partes se someterán a los juzgados y tribunales de Sevilla, salvo que una norma imperativa establezca otra jurisdicción.</p>
            </section>
          </template>

          <template v-else-if="legalSection === 'privacidad'">
            <p class="mb-6">Evently informa a los usuarios sobre el tratamiento de sus datos personales conforme al Reglamento (UE) 2016/679 (RGPD) y la Ley Orgánica 3/2018, de Protección de Datos Personales y garantía de los derechos digitales (LOPDGDD).</p>

            <section class="legal-section">
              <h3>1. Responsable del tratamiento</h3>
              <dl>
                <div><dt>Responsable</dt><dd>Evently</dd></div>
                <div><dt>Domicilio</dt><dd>Calle Américo Vespucio, 27, 41092 Sevilla, España</dd></div>
                <div><dt>Contacto</dt><dd><a :href="`mailto:${contactEmail}`">{{ contactEmail }}</a></dd></div>
              </dl>
            </section>

            <section class="legal-section">
              <h3>2. Datos y finalidades</h3>
              <p>Los datos que el usuario facilite voluntariamente podrán tratarse para estas finalidades:</p>
              <ul>
                <li>Gestionar el registro en la lista de preventa y notificar el acceso anticipado.</li>
                <li>Gestionar solicitudes de contacto o información enviadas a través de la web.</li>
                <li>Facilitar la conexión entre usuarios y proveedores de catering, fotografía y vídeo, floristería y decoración, o música y DJ.</li>
                <li>Enviar comunicaciones comerciales relacionadas con Evently, únicamente cuando exista consentimiento previo.</li>
              </ul>
            </section>

            <section class="legal-section">
              <h3>3. Legitimación</h3>
              <p>La base jurídica es el consentimiento expreso del interesado otorgado al marcar las casillas de aceptación habilitadas en los formularios de la web y al solicitar expresamente un servicio o información. Si el tratamiento fuera necesario para ejecutar una solicitud o obligación legal, se aplicará la legitimación correspondiente.</p>
            </section>

            <section class="legal-section">
              <h3>4. Destinatarios y comunicaciones</h3>
              <p>No se cederán datos personales a terceros salvo obligación legal o cuando sea imprescindible para prestar el servicio solicitado, por ejemplo, para compartir los datos mínimos necesarios con un proveedor elegido por el usuario. Los encargados de tratamiento seleccionados deberán garantizar medidas adecuadas de protección.</p>
            </section>

            <section class="legal-section">
              <h3>5. Conservación</h3>
              <p>Los datos se conservarán mientras exista la relación o solicitud, durante el tiempo necesario para cumplir las finalidades indicadas y los plazos legales aplicables. Los datos de comunicaciones comerciales se conservarán hasta que el usuario retire su consentimiento o solicite su supresión.</p>
            </section>

            <section class="legal-section">
              <h3>6. Derechos</h3>
              <p>El usuario puede solicitar el acceso, rectificación, supresión, limitación del tratamiento, portabilidad y oposición a sus datos mediante solicitud escrita al domicilio postal o al correo <a :href="`mailto:${contactEmail}`">{{ contactEmail }}</a>, indicando el derecho que desea ejercer y aportando una identificación suficiente.</p>
              <p>También puede retirar el consentimiento en cualquier momento, sin que ello afecte a la licitud del tratamiento realizado previamente. Si considera vulnerados sus derechos, puede presentar una reclamación ante la Agencia Española de Protección de Datos (AEPD), C. Jorge Juan, 6, 28001 Madrid, <a href="https://www.aepd.es" target="_blank" rel="noopener noreferrer">www.aepd.es</a>.</p>
            </section>

            <section class="legal-section">
              <h3>7. Seguridad</h3>
              <p>Evently aplica medidas técnicas y organizativas razonables para evitar la pérdida, uso indebido, acceso no autorizado o alteración de los datos personales. Ningún sistema ofrece seguridad absoluta, por lo que el usuario debe utilizar credenciales seguras y comunicar cualquier incidente sospechoso.</p>
            </section>
          </template>

          <template v-else>
            <p class="mb-6">Esta Política explica qué son las cookies y cómo se utilizan en <a :href="siteUrl" target="_blank" rel="noopener noreferrer" class="font-semibold underline">{{ siteUrl }}</a>. Puedes cambiar tus preferencias en cualquier momento desde «Personalizar Cookies» en el pie de página.</p>

            <section class="legal-section">
              <h3>1. Qué son las cookies</h3>
              <p>Las cookies son pequeños archivos que se almacenan en el dispositivo del usuario para recordar preferencias, habilitar funciones o recopilar información técnica y estadística sobre el uso de un sitio web.</p>
            </section>

            <section class="legal-section">
              <h3>2. Tipos utilizados</h3>
              <ul>
                <li><strong>Cookies técnicas necesarias:</strong> permiten navegar, recordar decisiones esenciales y utilizar funciones básicas. No requieren consentimiento cuando son estrictamente necesarias.</li>
                <li><strong>Cookies de análisis o estadística:</strong> ayudan a conocer de forma agregada cómo se utiliza Evently y a mejorar la experiencia. Solo se activarán si el usuario las acepta.</li>
                <li><strong>Enlaces y servicios externos:</strong> al abrir enlaces a Instagram o a sitios de proveedores, esos terceros pueden instalar sus propias cookies conforme a sus políticas. Evently no controla dichas cookies.</li>
              </ul>
            </section>

            <section class="legal-section">
              <h3>3. Cómo configurar las preferencias</h3>
              <p>Desde «Personalizar Cookies» puedes aceptar todas las categorías, rechazar las no esenciales o guardar una selección personalizada. La opción «Guardar preferencias» queda disponible para guardar tu decisión en este dispositivo.</p>
            </section>

            <section class="legal-section">
              <h3>4. Gestión desde el navegador</h3>
              <p>También puedes deshabilitar o eliminar cookies desde el navegador. La configuración puede variar por versión:</p>
              <ul>
                <li><strong>Google Chrome:</strong> Configuración → Privacidad y seguridad → Cookies y otros datos de sitios.</li>
                <li><strong>Safari:</strong> Preferencias/Ajustes → Privacidad → Gestionar datos de sitios web.</li>
                <li><strong>Mozilla Firefox:</strong> Ajustes → Privacidad y seguridad → Cookies y datos del sitio.</li>
                <li><strong>Microsoft Edge:</strong> Configuración → Cookies y permisos del sitio → Gestionar y eliminar cookies y datos del sitio.</li>
              </ul>
            </section>

            <section class="legal-section">
              <h3>5. Retirada del consentimiento</h3>
              <p>La retirada o modificación posterior no afecta a la licitud del tratamiento basado en el consentimiento previo a su retirada. Puedes borrar también las cookies almacenadas desde tu navegador.</p>
            </section>

            <button type="button" class="mt-2 w-full sm:w-auto bg-navy hover:bg-navyLight text-cream font-bold px-6 py-3 rounded-xl transition border border-gold/30" @click="openCookiePreferences">
              Personalizar Cookies
            </button>
          </template>
        </div>
      </article>
    </div>

    <div v-if="cookiePreferencesOpen" class="fixed inset-0 z-[70] bg-navy/85 backdrop-blur-sm flex items-center justify-center p-4" role="dialog" aria-modal="true" aria-labelledby="cookie-preferences-title" @click.self="closeCookiePreferences">
      <div class="bg-creamSoft text-navy rounded-2xl max-w-lg w-full p-6 sm:p-8 shadow-2xl relative border border-gold/50 max-h-[92vh] overflow-y-auto">
        <button type="button" class="absolute top-5 right-5 w-9 h-9 rounded-full bg-navy/10 hover:bg-navy hover:text-cream flex items-center justify-center font-bold" aria-label="Cerrar preferencias de cookies" @click="closeCookiePreferences">✕</button>
        <span class="text-goldDark text-xs font-bold uppercase tracking-widest">Centro de preferencias</span>
        <h2 id="cookie-preferences-title" class="font-serif text-3xl font-bold mt-1 mb-3 pr-10">Personalizar Cookies</h2>
        <p class="text-sm text-navy/75 mb-6">Elige qué categorías no esenciales quieres permitir. Las cookies técnicas necesarias siempre permanecen activas.</p>

        <div class="space-y-3 mb-6">
          <div class="flex items-center justify-between gap-4 bg-cream border border-navy/10 rounded-xl p-4">
            <div>
              <h3 class="font-bold text-sm">Técnicas necesarias</h3>
              <p class="text-xs text-navy/65 mt-1">Funcionamiento básico y seguridad.</p>
            </div>
            <span class="shrink-0 text-xs font-bold bg-navy text-cream px-3 py-1.5 rounded-full">Siempre activas</span>
          </div>
          <label class="flex items-center justify-between gap-4 bg-cream border border-navy/10 rounded-xl p-4 cursor-pointer">
            <span>
              <span class="font-bold text-sm block">Análisis y estadísticas</span>
              <span class="text-xs text-navy/65 mt-1 block">Medición agregada para mejorar la web.</span>
            </span>
            <input v-model="analyticsCookies" type="checkbox" class="w-5 h-5 accent-navy shrink-0" aria-label="Permitir cookies de análisis">
          </label>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2.5">
          <button type="button" class="bg-navy text-cream font-bold py-3 rounded-xl hover:bg-navyLight transition" @click="acceptAllCookies">Aceptar todas</button>
          <button type="button" class="border border-navy/20 text-navy font-bold py-3 rounded-xl hover:bg-navy/5 transition" @click="rejectOptionalCookies">Rechazar no esenciales</button>
          <button type="button" class="sm:col-span-2 bg-gold text-navy font-bold py-3 rounded-xl hover:bg-goldDark transition" @click="saveCookiePreferences">Guardar preferencias</button>
        </div>
      </div>
    </div>

    <div v-if="detailModalOpen" class="fixed inset-0 z-50 bg-navy/75 backdrop-blur-sm flex items-center justify-center p-4" @click.self="closeDetailModal">
      <div class="bg-creamSoft text-navy rounded-2xl max-w-lg w-full p-6 sm:p-8 shadow-2xl relative max-h-[90vh] overflow-y-auto">
        <button type="button" class="absolute top-5 right-5 w-8 h-8 rounded-full bg-navy/10 hover:bg-navy hover:text-cream flex items-center justify-center font-bold" @click="closeDetailModal">✕</button>
        <template v-if="detailProvider">
          <span class="text-xs font-bold uppercase tracking-wider text-goldDark">{{ detailProvider.category }} · {{ detailProvider.city }}</span>
          <h3 class="font-serif text-2xl font-bold text-navy mt-1 mb-2">{{ detailProvider.name }}</h3>
          <p class="text-sm text-navy/80 mb-5">{{ detailProvider.summary }}</p>
          <p v-if="detailProvider.location" class="text-xs text-navy/70 mb-3">📍 {{ detailProvider.location }}</p>
          <a v-if="detailProvider.website?.startsWith('http')" :href="detailProvider.website" target="_blank" rel="noopener noreferrer" class="inline-block text-xs font-bold text-goldDark underline mb-5">
            Visitar web oficial
          </a>
          <p v-else-if="detailProvider.website" class="text-xs text-navy/70 mb-5">Instagram: {{ detailProvider.website }}</p>

          <div class="bg-cream rounded-xl p-4 mb-5 border border-gold/40">
            <h4 class="text-xs font-bold uppercase tracking-wider text-navy mb-2">Qué incluye exactamente este precio:</h4>
            <ul class="space-y-1.5 text-sm">
              <li v-for="item in detailProvider.includes" :key="item" class="flex items-center gap-2"><span class="text-goldDark font-bold">✓</span> {{ item }}</li>
            </ul>
            <p class="text-xs text-navy/60 mt-3 pt-2 border-t border-navy/10">Cancelación: <strong>{{ detailProvider.cancellation }}</strong></p>
          </div>

          <div v-if="detailProvider.pricingType === 'per_person'" class="mb-5">
            <div class="flex justify-between text-xs font-bold mb-1">
              <span>Número de invitados: <span class="text-goldDark text-sm">{{ modalGuests }} personas</span></span>
              <span>{{ detailProvider.basePrice }} €/persona</span>
            </div>
            <input v-model.number="modalGuests" type="range" :min="detailProvider.minUnits" max="100" class="w-full accent-navy cursor-pointer">
          </div>

          <div class="flex items-center justify-between bg-navy text-cream p-4 rounded-xl mb-5">
            <div>
              <span class="text-xs text-cream/70 block">Precio final cerrado</span>
              <span class="font-serif text-2xl font-bold text-gold">{{ detailTotal }} €</span>
            </div>
            <span class="text-xs bg-white/10 px-3 py-1 rounded-full">IVA incluido</span>
          </div>

          <button type="button" class="w-full bg-gold hover:bg-goldDark text-navy font-bold py-3 rounded-xl transition" @click="addToEvent">
            Añadir a Mi Evento
          </button>
        </template>
      </div>
    </div>

    <div v-if="preventaModalOpen" class="fixed inset-0 z-50 bg-navy/75 backdrop-blur-sm flex items-center justify-center p-4" @click.self="closePreventaModal">
      <div class="bg-navy text-cream rounded-2xl max-w-md w-full p-7 sm:p-8 shadow-2xl relative border border-gold/40">
        <button type="button" class="absolute top-5 right-5 w-8 h-8 rounded-full bg-white/10 hover:bg-gold hover:text-navy flex items-center justify-center font-bold" @click="closePreventaModal">✕</button>
        <div v-if="!preventaSubmitted">
          <span class="text-gold text-xs font-bold uppercase tracking-widest">Lista VIP Preventa</span>
          <h3 class="font-serif text-3xl font-bold mt-1 mb-2">Estrena Evently antes que nadie</h3>
          <p class="text-sm text-cream/80 mb-6">Apúntate gratis y consigue prioridad de reserva, tarifas cerradas verificadas y 0 € de gastos de gestión en tu primer evento.</p>
          <form class="space-y-4" @submit.prevent="submitPreventa">
            <div>
              <label class="block text-xs font-semibold uppercase tracking-wider text-gold mb-1">Tu nombre</label>
              <input v-model="preventaName" type="text" required placeholder="Ej. Laura Gómez" class="w-full bg-navyLight border border-white/20 rounded-xl px-4 py-2.5 text-sm text-cream placeholder:text-cream/40 focus:outline-none focus:border-gold">
            </div>
            <div>
              <label class="block text-xs font-semibold uppercase tracking-wider text-gold mb-1">Correo electrónico</label>
              <input v-model="preventaEmail" type="email" required placeholder="laura@email.com" class="w-full bg-navyLight border border-white/20 rounded-xl px-4 py-2.5 text-sm text-cream placeholder:text-cream/40 focus:outline-none focus:border-gold">
            </div>
            <div>
              <label class="block text-xs font-semibold uppercase tracking-wider text-gold mb-1">Próxima celebración</label>
              <select v-model="preventaEvent" class="w-full bg-navyLight border border-white/20 rounded-xl px-4 py-2.5 text-sm text-cream focus:outline-none focus:border-gold">
                <option>Cena de Navidad / Fin de año</option>
                <option>Cumpleaños o fiesta privada</option>
                <option>Boda, bautizo o comunión</option>
                <option>Graduación o evento universitario</option>
              </select>
            </div>
            <button type="submit" class="w-full bg-gold hover:bg-goldDark text-navy font-bold py-3 rounded-xl transition mt-2">Unirme gratis a la preventa ✨</button>
          </form>
        </div>
        <div v-else class="text-center py-6">
          <div class="w-14 h-14 rounded-full bg-gold text-navy text-2xl font-bold flex items-center justify-center mx-auto mb-4">✓</div>
          <h3 class="font-serif text-2xl font-bold mb-2">¡Ya estás dentro de la lista!</h3>
          <p class="text-sm text-cream/80 mb-6">Te avisaremos con acceso anticipado para que organices tu próxima celebración con precios reales y cero dramas.</p>
          <button type="button" class="bg-cream text-navy font-bold px-6 py-2.5 rounded-full text-sm" @click="closePreventaModal">Volver a Evently</button>
        </div>
      </div>
    </div>

    <div v-if="toastMessage" class="fixed bottom-5 right-5 z-50 bg-navy text-cream border border-gold px-5 py-3 rounded-xl shadow-xl text-sm font-medium">
      {{ toastMessage }}
    </div>
  </div>
</template>

<script setup lang="ts">
interface Provider {
  id: number
  name: string
  category: string
  city: string
  eventType: string
  rating: string
  reviews: number
  pricingType: 'per_person' | 'fixed'
  basePrice: number
  unitLabel: string
  minUnits: number
  defaultUnits: number
  image: string
  summary: string
  includes: string[]
  cancellation: string
  location?: string
  website?: string
}

interface SelectedService {
  uid: number
  id: number
  name: string
  category: string
  detailNote: string
  total: number
}

const activeTab = ref('explorar')
const filterCategory = ref('Todas')
const filterCity = ref('Todas')
const filterEvent = ref('Todos')
const compareList = ref<number[]>([])
const selectedServices = ref<SelectedService[]>([])
const detailModalOpen = ref(false)
const detailProvider = ref<Provider | null>(null)
const modalGuests = ref(20)
const preventaModalOpen = ref(false)
const preventaSubmitted = ref(false)
const preventaName = ref('')
const preventaEmail = ref('')
const preventaEvent = ref('Cena de Navidad / Fin de año')
const toastMessage = ref('')
let toastTimer: ReturnType<typeof setTimeout> | undefined

const siteUrl = 'https://evently-psi-ten.vercel.app'
const contactEmail = 'hola@evently.es'
type LegalSection = 'aviso' | 'privacidad' | 'cookies'
const legalModalOpen = ref(false)
const legalSection = ref<LegalSection>('aviso')
const cookiePreferencesOpen = ref(false)
const analyticsCookies = ref(false)

const legalModalTitleId = 'evently-legal-title'
const legalTitle = computed(() => {
  if (legalSection.value === 'aviso') return 'Aviso Legal'
  if (legalSection.value === 'privacidad') return 'Política de Privacidad'
  return 'Política de Cookies'
})

const providers: Provider[] = [
  { id: 1, name: 'ALDA & TERRY CATERING', category: 'Catering', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 48, pricingType: 'per_person', basePrice: 115, unitLabel: '€/persona', minUnits: 10, defaultUnits: 50, image: 'https://images.unsplash.com/photo-1555244162-803834f70033?auto=format&fit=crop&w=800&q=80', summary: 'Alta gastronomía andaluza con técnicas contemporáneas y puesta en escena de diseño para bodas exclusivas y celebraciones corporativas.', includes: ['Alta cocina andaluza', 'Menú personalizado', 'Puesta en escena de diseño', 'Servicio adaptado al evento'], cancellation: 'Consultar condiciones con el proveedor', location: 'Av. Manuel Siurot, 52, Sevilla', website: 'https://aldayterrycatering.com/' },
  { id: 2, name: 'Catering PROBOCA', category: 'Catering', city: 'Salteras', eventType: 'Boda', rating: '4.8', reviews: 36, pricingType: 'per_person', basePrice: 85, unitLabel: '€/persona', minUnits: 10, defaultUnits: 50, image: 'https://images.unsplash.com/photo-1414235077428-338989a2e8c0?auto=format&fit=crop&w=800&q=80', summary: 'Organización gastronómica de bodas y grandes eventos con materia prima de proximidad, estaciones temáticas y servicio integral.', includes: ['Estaciones gastronómicas', 'Cóctel de bienvenida', 'Servicio integral de bodas', 'Producto de proximidad'], cancellation: 'Consultar condiciones con el proveedor', location: 'Pol. Ind. Los Llanos, Calle A, Nave 4, Salteras', website: 'https://probocacatering.com/' },
  { id: 3, name: 'Santacruz Catering', category: 'Catering', city: 'Sevilla', eventType: 'Boda', rating: '4.8', reviews: 31, pricingType: 'per_person', basePrice: 80, unitLabel: '€/persona', minUnits: 10, defaultUnits: 50, image: 'https://images.unsplash.com/photo-1464366400600-7168b8af9bc3?auto=format&fit=crop&w=800&q=80', summary: 'Menús versátiles que equilibran recetas clásicas sevillanas y cócteles creativos con menaje cuidado.', includes: ['Catering finger food', 'Banquete a medida', 'Menaje incluido', 'Decoración de mesas'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Artesa, 8, Sevilla', website: 'http://www.santacruzcatering.es/' },
  { id: 4, name: 'Deseo Y Ginebra', category: 'Catering', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 27, pricingType: 'fixed', basePrice: 650, unitLabel: 'Por evento', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1470337458703-46ad1756a187?auto=format&fit=crop&w=800&q=80', summary: 'Barras móviles y coctelería de autor con combinados botánicos y bartenders profesionales para celebraciones privadas y de empresa.', includes: ['Coctelería de autor', 'Barra libre premium', 'Barra móvil temática', 'Bartenders profesionales'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Pino Central, Parcela 20, Nave 16, Sevilla', website: 'https://www.deseoyginebra.es/' },
  { id: 5, name: 'JUANMA & JULIÁN', category: 'Fotografía', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 42, pricingType: 'fixed', basePrice: 1800, unitLabel: 'Reportaje completo', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1511285560929-80b456fea0bc?auto=format&fit=crop&w=800&q=80', summary: 'Fotoperiodismo documental y estética editorial luminosa sin posados forzados para capturar emociones auténticas.', includes: ['Fotografía documental', 'Estética editorial', 'Reportaje preboda', 'Reportaje postboda'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle José Saramago, 11, Sevilla', website: 'https://www.juanmayjulian.com/' },
  { id: 6, name: 'Gabriel Navas', category: 'Fotografía', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 35, pricingType: 'fixed', basePrice: 1500, unitLabel: 'Reportaje completo', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1520854221256-17451cc331bf?auto=format&fit=crop&w=800&q=80', summary: 'Reportajes nupciales y eventos sociales con luz natural, mirada íntima y tratamiento artístico atemporal.', includes: ['Reportaje fotográfico integral', 'Edición artística atemporal', 'Álbum artesanal', 'Cobertura discreta'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Fresa, 10, Sevilla', website: 'https://www.gabrielnavas.com/' },
  { id: 7, name: 'Iván Pérez Fotógrafo', category: 'Fotografía', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 39, pricingType: 'fixed', basePrice: 1600, unitLabel: 'Foto y vídeo', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1507501336603-6e31db2be093?auto=format&fit=crop&w=800&q=80', summary: 'Fotografía y cinematografía nupcial con narrativa visual cinematográfica y una cobertura audiovisual completa.', includes: ['Fotografía y vídeo cinematográfico', 'Tomas aéreas con dron', 'Cobertura completa', 'Entrega de alta calidad'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Castillo de Constantina, 8, Sevilla', website: 'https://www.ipfotos.com/' },
  { id: 8, name: 'Raul Pilato', category: 'Fotografía', city: 'Sevilla', eventType: 'Boda', rating: '4.8', reviews: 29, pricingType: 'fixed', basePrice: 1350, unitLabel: 'Foto y vídeo', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1519741497674-611481863552?auto=format&fit=crop&w=800&q=80', summary: 'Fotografía y vídeo natural centrados en celebraciones con un enfoque cercano y sin interrupciones.', includes: ['Fotografía espontánea', 'Vídeo documental de boda', 'Entrega digital rápida', 'Cobertura discreta'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Felipe II, 13, Sevilla', website: 'https://fotografosevilla.es/' },
  { id: 9, name: 'Cártamo Flores', category: 'Floristería', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 44, pricingType: 'fixed', basePrice: 1200, unitLabel: 'Boda completa', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1519225421980-715cb0215aed?auto=format&fit=crop&w=800&q=80', summary: 'Diseño floral de referencia con atmósferas silvestres, orgánicas y sofisticadas, desde ramos hasta instalaciones completas.', includes: ['Diseño floral a medida', 'Instalaciones aéreas y altares', 'Ramos de novia de autor', 'Decoración integral'], cancellation: 'Consultar condiciones con el proveedor', location: 'Av. de Cádiz, 13, Sevilla', website: 'https://www.cartamoflores.com/' },
  { id: 10, name: 'Concha Candela Arte Floral', category: 'Floristería', city: 'Sevilla', eventType: 'Boda', rating: '4.8', reviews: 33, pricingType: 'fixed', basePrice: 1000, unitLabel: 'Boda completa', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1507501336603-6e31db2be093?auto=format&fit=crop&w=800&q=80', summary: 'Elegancia clásica renovada y sensibilidad cromática para iglesias, patios sevillanos y haciendas.', includes: ['Decoración floral de templos', 'Centros de mesa y banquetes', 'Asesoramiento estético integral', 'Flor fresca de temporada'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Virgen de Regla, 22, Sevilla', website: 'http://www.conchacandela.com/' },
  { id: 11, name: 'BlancoPuro', category: 'Floristería', city: 'Sevilla', eventType: 'Boda', rating: '4.8', reviews: 26, pricingType: 'fixed', basePrice: 600, unitLabel: 'Por montaje', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1492684223066-81342ee5ff30?auto=format&fit=crop&w=800&q=80', summary: 'Alquiler de mobiliario de diseño, rincones chill-out e iluminación decorativa para transformar espacios singulares.', includes: ['Mobiliario de diseño', 'Iluminación ambiental y guirnaldas', 'Rincones chill-out', 'Seating y escenografía'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Jesús del Gran Poder, 44, Sevilla', website: 'https://blancopuro.com/' },
  { id: 12, name: 'Mob4Events', category: 'Floristería', city: 'Sevilla', eventType: 'Boda', rating: '4.7', reviews: 24, pricingType: 'fixed', basePrice: 500, unitLabel: 'Por pedido', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1507501336603-6e31db2be093?auto=format&fit=crop&w=800&q=80', summary: 'Logística y mobiliario para celebraciones con sillería, mesas imperiales, barras y estructuras adaptables.', includes: ['Sillería y mesas para banquetes', 'Mobiliario lounge y barras', 'Montaje integral', 'Logística en Andalucía'], cancellation: 'Consultar condiciones con el proveedor', location: 'Pol. Ind. Store, Calle Gramil, 11, Sevilla', website: 'https://mob4events.es/' },
  { id: 13, name: 'Bumburay', category: 'Música', city: 'Sevilla', eventType: 'Boda', rating: '4.9', reviews: 32, pricingType: 'fixed', basePrice: 1200, unitLabel: 'Servicio completo', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1470225620780-dba8ba36b745?auto=format&fit=crop&w=800&q=80', summary: 'Banda sevillana con versiones pop-rock y clásicos españoles e internacionales para animar la pista y la fiesta.', includes: ['Música en directo para bodas', 'Repertorio pop-rock', 'Clásicos españoles e internacionales', 'Sonido propio incluido'], cancellation: 'Consultar condiciones con el proveedor', location: 'Sevilla capital y provincia', website: '@bumburayoficial' },
  { id: 14, name: 'Aflamenk2s Grupo Flamenco', category: 'Música', city: 'Sevilla', eventType: 'Boda', rating: '4.8', reviews: 28, pricingType: 'fixed', basePrice: 750, unitLabel: 'Actuación', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1507676184212-d03ab07a01bf?auto=format&fit=crop&w=800&q=80', summary: 'Flamenco-pop, rumbas y versiones aflamencadas para cócteles y celebraciones con sello auténtico de Sevilla.', includes: ['Flamenco-pop en directo', 'Amenización de cóctel', 'Rumbas y sevillanas', 'Formato cercano y festivo'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Asunción, Sevilla', website: '@aflamenk2s' },
  { id: 15, name: 'Rockadelia', category: 'Música', city: 'Sevilla', eventType: 'Graduación', rating: '4.8', reviews: 37, pricingType: 'fixed', basePrice: 550, unitLabel: 'Servicio de DJ', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1571266028243-d220c9c3b3b6?auto=format&fit=crop&w=800&q=80', summary: 'DJs, sonido profesional e iluminación espectacular con sesiones musicales personalizadas y efectos técnicos.', includes: ['DJ profesional', 'Sonorización de ceremonias', 'Iluminación de pista', 'Efectos arquitectónicos'], cancellation: 'Consultar condiciones con el proveedor', location: 'Calle Torneo, Sevilla', website: 'https://rockadelia.com/' },
  { id: 16, name: 'INFERNO INC. EVENTOS', category: 'Música', city: 'Sevilla', eventType: 'Graduación', rating: '4.9', reviews: 30, pricingType: 'fixed', basePrice: 700, unitLabel: 'Producción técnica', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1492684223066-81342ee5ff30?auto=format&fit=crop&w=800&q=80', summary: 'Producción técnica de sonido, iluminación escénica y DJ para celebraciones con montajes audiovisuales cuidados.', includes: ['Producción audiovisual e iluminación', 'Show DJ y animación', 'Sonido de alta potencia', 'Cabina de diseño'], cancellation: 'Consultar condiciones con el proveedor', location: 'Mairena del Aljarafe, Sevilla', website: 'https://infernobodas.es/' },
  { id: 17, name: 'Hacienda de Orán', category: 'Espacios', city: 'Utrera', eventType: 'Boda', rating: '4.9', reviews: 46, pricingType: 'fixed', basePrice: 4500, unitLabel: 'Alquiler del espacio', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1519167758481-83f550bb49b3?auto=format&fit=crop&w=800&q=80', summary: 'Hacienda olivarera del siglo XVII con jardines históricos, patios empedrados, salones señoriales y alojamiento propio.', includes: ['Patios andaluces y jardines', 'Salones históricos de gran aforo', 'Alojamiento boutique', 'Instalaciones ecuestres'], cancellation: 'Consultar condiciones con el proveedor', location: 'Carretera Utrera - Los Palacios, Km 4, Utrera', website: 'http://haciendadeoran.com/' },
  { id: 18, name: 'Hacienda de Medina', category: 'Espacios', city: 'Carmona', eventType: 'Boda', rating: '4.9', reviews: 52, pricingType: 'fixed', basePrice: 4000, unitLabel: 'Alquiler del espacio', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1519225421980-715cb0215aed?auto=format&fit=crop&w=800&q=80', summary: 'Hacienda histórica en Carmona con patios de albero, porches porticados y jardines iluminados para eventos exclusivos.', includes: ['Patios tradicionales sevillanos', 'Espacios interiores y exteriores', 'Exclusividad por evento', 'Entorno histórico'], cancellation: 'Consultar condiciones con el proveedor', location: 'Autovía A-4, Km 514, Carmona', website: 'https://www.haciendademedina.es/' },
  { id: 19, name: 'Hacienda El Vizir', category: 'Espacios', city: 'Espartinas', eventType: 'Boda', rating: '4.8', reviews: 41, pricingType: 'fixed', basePrice: 3500, unitLabel: 'Alquiler del espacio', minUnits: 1, defaultUnits: 1, image: 'https://images.unsplash.com/photo-1464366400600-7168b8af9bc3?auto=format&fit=crop&w=800&q=80', summary: 'Enclave mudéjar cerca de Sevilla con patios ajardinados, plaza propia y salones climatizados de gran aforo.', includes: ['Arquitectura mudéjar y patios', 'Salones climatizados', 'Gran aforo', 'Cercanía a Sevilla'], cancellation: 'Consultar condiciones con el proveedor', location: 'Carretera Espartinas - Villanueva del Ariscal, Km 1', website: 'https://haciendaelvizir.com/' }
]

const howItWorks = [
  { title: 'Conecta con precios reales', text: 'Filtra por ciudad, fecha y tipo de celebración viendo desde el principio cuánto cuesta cada servicio sin formularios eternos.' },
  { title: 'Compara sin hojas de Excel', text: 'Selecciona hasta 3 proveedores y pon sus paquetes lado a lado para revisar qué incluye cada uno y elegir con total seguridad.' },
  { title: 'Reserva y celebra', text: 'Añade tus servicios a «Mi Evento», controla el presupuesto total en tiempo real y bloquea tu fecha en un solo clic.' }
]

const tasks = ref([
  { id: 1, text: 'Definir lista aproximada de invitados', done: true },
  { id: 2, text: 'Comparar opciones de catering con precio cerrado', done: false },
  { id: 3, text: 'Bloquear fotógrafo antes de que suba la demanda', done: false }
])
const newTask = ref('')

const filteredProviders = computed(() => providers.filter(provider =>
  (filterCategory.value === 'Todas' || provider.category === filterCategory.value) &&
  (filterCity.value === 'Todas' || provider.city === filterCity.value) &&
  (filterEvent.value === 'Todos' || provider.eventType === filterEvent.value)
))

const comparedProviders = computed(() => compareList.value.map(id => providers.find(provider => provider.id === id)).filter((provider): provider is Provider => Boolean(provider)))
const eventTotal = computed(() => selectedServices.value.reduce((total, service) => total + service.total, 0))
const detailTotal = computed(() => detailProvider.value ? (detailProvider.value.pricingType === 'per_person' ? detailProvider.value.basePrice * modalGuests.value : detailProvider.value.basePrice) : 0)

function switchTab(tab: string) {
  activeTab.value = tab
  if (import.meta.client) window.scrollTo({ top: 0, behavior: 'smooth' })
}

function resetFilters() {
  filterCategory.value = 'Todas'
  filterCity.value = 'Todas'
  filterEvent.value = 'Todos'
}

function toggleCompare(id: number) {
  if (compareList.value.includes(id)) {
    compareList.value = compareList.value.filter(item => item !== id)
    showToast('Eliminado del comparador')
  } else {
    if (compareList.value.length >= 3) {
      showToast('Máximo 3 proveedores en el comparador')
      return
    }
    compareList.value.push(id)
    showToast('Añadido al comparador')
  }
}

function openDetailModal(provider: Provider) {
  detailProvider.value = provider
  modalGuests.value = provider.defaultUnits
  detailModalOpen.value = true
}

function closeDetailModal() {
  detailModalOpen.value = false
}

function addToEvent() {
  if (!detailProvider.value) return
  selectedServices.value.push({
    uid: Date.now(),
    id: detailProvider.value.id,
    name: detailProvider.value.name,
    category: detailProvider.value.category,
    detailNote: detailProvider.value.pricingType === 'per_person' ? `${modalGuests.value} personas` : detailProvider.value.unitLabel,
    total: detailTotal.value
  })
  closeDetailModal()
  showToast(`«${detailProvider.value.name}» añadido a Mi Evento`)
}

function removeService(uid: number) {
  selectedServices.value = selectedServices.value.filter(service => service.uid !== uid)
}

function addTask() {
  const text = newTask.value.trim()
  if (!text) return
  tasks.value.push({ id: Date.now(), text, done: false })
  newTask.value = ''
}

function simulateBooking() {
  if (selectedServices.value.length === 0) {
    showToast('Añade primero al menos un servicio a tu evento')
    return
  }
  openPreventaModal()
}

function openPreventaModal() {
  preventaSubmitted.value = false
  preventaModalOpen.value = true
}

function closePreventaModal() {
  preventaModalOpen.value = false
}

function openLegalModal(section: LegalSection) {
  legalSection.value = section
  legalModalOpen.value = true
  if (import.meta.client) document.body.style.overflow = 'hidden'
}

function closeLegalModal() {
  legalModalOpen.value = false
  if (!cookiePreferencesOpen.value && import.meta.client) document.body.style.overflow = ''
}

function openCookiePreferences() {
  cookiePreferencesOpen.value = true
  if (import.meta.client) document.body.style.overflow = 'hidden'
}

function closeCookiePreferences() {
  cookiePreferencesOpen.value = false
  if (!legalModalOpen.value && import.meta.client) document.body.style.overflow = ''
}

function acceptAllCookies() {
  analyticsCookies.value = true
  saveCookiePreferences()
}

function rejectOptionalCookies() {
  analyticsCookies.value = false
  saveCookiePreferences()
}

function saveCookiePreferences() {
  if (import.meta.client) {
    localStorage.setItem('evently_cookie_preferences', JSON.stringify({
      necessary: true,
      analytics: analyticsCookies.value,
      updatedAt: new Date().toISOString()
    }))
  }
  closeCookiePreferences()
  showToast('Preferencias de cookies guardadas')
}

async function submitPreventa() {
  try {
    await $fetch('/api/preventa', {
      method: 'POST',
      body: {
        name: preventaName.value,
        email: preventaEmail.value,
        celebration: preventaEvent.value
      }
    })
    preventaSubmitted.value = true
  } catch (error: unknown) {
    const message = error && typeof error === 'object' && 'data' in error
      ? String((error as { data?: { statusMessage?: string } }).data?.statusMessage || 'No se pudo completar el registro')
      : 'No se pudo completar el registro'
    showToast(message)
  }
}

function showToast(message: string) {
  toastMessage.value = message
  if (toastTimer) clearTimeout(toastTimer)
  toastTimer = setTimeout(() => {
    toastMessage.value = ''
  }, 2800)
}

onMounted(() => {
  if (!import.meta.client) return
  const stored = localStorage.getItem('evently_cookie_preferences')
  if (!stored) return
  try {
    const preferences = JSON.parse(stored) as { analytics?: boolean }
    analyticsCookies.value = preferences.analytics === true
  } catch {
    localStorage.removeItem('evently_cookie_preferences')
  }
})
</script>

<style>
body {
  background-color: #FFFCBA;
  color: #071932;
  font-family: 'Plus Jakarta Sans', sans-serif;
}
.gold-border {
  border: 1px solid rgba(255, 252, 186, 0.35);
}
.legal-content .legal-section {
  margin-bottom: 1.75rem;
}
.legal-content .legal-section h3 {
  font-family: 'Playfair Display', serif;
  color: #071932;
  font-size: 1.25rem;
  font-weight: 700;
  margin-bottom: 0.65rem;
  padding-bottom: 0.4rem;
  border-bottom: 1px solid rgba(7, 25, 50, 0.12);
}
.legal-content .legal-section p + p,
.legal-content .legal-section p + ul,
.legal-content .legal-section ul + p {
  margin-top: 0.75rem;
}
.legal-content ul {
  list-style: disc;
  padding-left: 1.25rem;
}
.legal-content a {
  color: #071932;
  text-decoration: underline;
  text-underline-offset: 3px;
}
.legal-content dl {
  display: grid;
  gap: 0.5rem;
  margin-bottom: 1rem;
  background: #FFFCBA;
  border: 1px solid rgba(7, 25, 50, 0.1);
  border-radius: 0.75rem;
  padding: 1rem;
}
.legal-content dl div {
  display: grid;
  grid-template-columns: minmax(0, 180px) 1fr;
  gap: 0.75rem;
}
.legal-content dt {
  font-weight: 700;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}
@media (max-width: 640px) {
  .legal-content dl div {
    grid-template-columns: 1fr;
    gap: 0;
  }
}
</style>
