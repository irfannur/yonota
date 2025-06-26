<template>
  <div class="max-w-3xl mx-auto p-6" :class="{ 'dark': isDarkMode }">
    <div class="bg-white dark:bg-gray-800 p-6 rounded-2xl shadow-md border border-blue-200 dark:border-blue-700">
      <div class="flex justify-between items-center mb-6">
        <h1
          class="text-3xl font-extrabold text-blue-700 dark:text-blue-300 tracking-wide drop-shadow flex items-center gap-2">
          <img class="h-11 w-auto" src="/yonota-logo.png" alt="Logo">
          <span>Yonota</span>
        </h1>
        <div class="flex gap-2">
          <button @click="toggleTheme"
            class="text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2"><svg
              data-toggle-icon="sun" class="w-3.5 h-3.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
              fill="currentColor" viewBox="0 0 20 20">
              <path
                d="M10 15a5 5 0 1 0 0-10 5 5 0 0 0 0 10Zm0-11a1 1 0 0 0 1-1V1a1 1 0 0 0-2 0v2a1 1 0 0 0 1 1Zm0 12a1 1 0 0 0-1 1v2a1 1 0 1 0 2 0v-2a1 1 0 0 0-1-1ZM4.343 5.757a1 1 0 0 0 1.414-1.414L4.343 2.929a1 1 0 0 0-1.414 1.414l1.414 1.414Zm11.314 8.486a1 1 0 0 0-1.414 1.414l1.414 1.414a1 1 0 0 0 1.414-1.414l-1.414-1.414ZM4 10a1 1 0 0 0-1-1H1a1 1 0 0 0 0 2h2a1 1 0 0 0 1-1Zm15-1h-2a1 1 0 1 0 0 2h2a1 1 0 0 0 0-2ZM4.343 14.243l-1.414 1.414a1 1 0 1 0 1.414 1.414l1.414-1.414a1 1 0 0 0-1.414-1.414ZM14.95 6.05a1 1 0 0 0 .707-.293l1.414-1.414a1 1 0 1 0-1.414-1.414l-1.414 1.414a1 1 0 0 0 .707 1.707Z">
              </path>
            </svg></button>
        </div>
      </div>
      <div class="grid gap-4 mb-6 sm:grid-cols-2">
        <input v-model="storeName" type="text" placeholder="Nama Toko" class="input" />
        <input v-model="transactionDate" type="date" class="input" />
        <input v-model="storeAddress" type="text" placeholder="Alamat Toko" class="input sm:col-span-2" />
        <input v-model="customerName" type="text" placeholder="Pelanggan" class="input sm:col-span-2" />
      </div>

      <div class="mb-6">
        <h2
          class="text-lg font-semibold mb-2 text-gray-700 dark:text-gray-300 border-b border-blue-200 dark:border-blue-700 pb-1">
          Produk</h2>
        <div v-for="(item, index) in products" :key="index"
          class="grid grid-cols-1 sm:grid-cols-5 gap-2 mb-2 items-center w-full bg-blue-50 dark:bg-gray-700 rounded-lg p-2">
          <input v-model="item.name" placeholder="Nama Produk" class="input sm:col-span-2 w-full" />
          <input v-model.number="item.qty" type="number" placeholder="Jml" class="input w-full" min="1" />
          <input v-model="item.priceDisplay" @input="updatePrice(index)" placeholder="Harga" class="input w-full" />
          <div class="flex justify-end w-full">
            <button @click="removeProduct(index)"
              class="btn btn-red flex items-center justify-center h-9 w-9 rounded-full shadow hover:scale-110 transition-transform duration-150"
              title="Remove Product" aria-label="Remove Product">
              <span class="text-lg leading-none">✕</span>
            </button>
          </div>
        </div>
      </div>

      <div class="flex flex-wrap gap-2 mb-6">
        <button @click="addProduct"
          class="text-white bg-gradient-to-r from-cyan-500 to-blue-500 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-cyan-300 dark:focus:ring-cyan-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2">+
          Add Product</button>
        <div class="flex-1 flex justify-end">

        </div>
      </div>
    </div>

    <div
      class="mt-8 bg-gradient-to-br from-blue-100 via-white to-blue-50 dark:from-gray-900 dark:via-gray-800 dark:to-gray-900 p-6 rounded-xl border border-blue-200 dark:border-blue-700 shadow-lg">

      <div class="flex flex-wrap gap-2 mb-6">
        <h3 class="text-2xl font-bold text-center text-gray-800 dark:text-gray-200 mb-4 tracking-wide">Invoice Preview
        </h3>
        <div class="flex-1 flex justify-end">
          <button @click="generatePDF"
            class="text-white bg-gradient-to-r from-teal-400 via-teal-500 to-teal-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-teal-300 dark:focus:ring-teal-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2">
            <svg class="w-[25px] h-[25px] text-gray-800 dark:text-white" aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
              <path fill-rule="evenodd"
                d="M9 2.221V7H4.221a2 2 0 0 1 .365-.5L8.5 2.586A2 2 0 0 1 9 2.22ZM11 2v5a2 2 0 0 1-2 2H4a2 2 0 0 0-2 2v7a2 2 0 0 0 2 2 2 2 0 0 0 2 2h12a2 2 0 0 0 2-2 2 2 0 0 0 2-2v-7a2 2 0 0 0-2-2V4a2 2 0 0 0-2-2h-7Zm-6 9a1 1 0 0 0-1 1v5a1 1 0 1 0 2 0v-1h.5a2.5 2.5 0 0 0 0-5H5Zm1.5 3H6v-1h.5a.5.5 0 0 1 0 1Zm4.5-3a1 1 0 0 0-1 1v5a1 1 0 0 0 1 1h1.376A2.626 2.626 0 0 0 15 15.375v-1.75A2.626 2.626 0 0 0 12.375 11H11Zm1 5v-3h.375a.626.626 0 0 1 .625.626v1.748a.625.625 0 0 1-.626.626H12Zm5-5a1 1 0 0 0-1 1v5a1 1 0 1 0 2 0v-1h1a1 1 0 1 0 0-2h-1v-1h1a1 1 0 1 0 0-2h-2Z"
                clip-rule="evenodd" />
            </svg>
          </button>
          <button @click="generateImage"
            class="text-white bg-gradient-to-br from-purple-600 to-blue-500 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center me-2 mb-2"><svg
              class="w-[25px] h-[25px] text-gray-800 dark:text-white" aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
              <path fill-rule="evenodd"
                d="M9 2.221V7H4.221a2 2 0 0 1 .365-.5L8.5 2.586A2 2 0 0 1 9 2.22ZM11 2v5a2 2 0 0 1-2 2H4v11a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2h-7Zm.394 9.553a1 1 0 0 0-1.817.062l-2.5 6A1 1 0 0 0 8 19h8a1 1 0 0 0 .894-1.447l-2-4A1 1 0 0 0 13.2 13.4l-.53.706-1.276-2.553ZM13 9.5a1.5 1.5 0 1 1 3 0 1.5 1.5 0 0 1-3 0Z"
                clip-rule="evenodd" />
            </svg>
          </button>
        </div>
      </div>

      <div ref="invoice" class="bg-white p-6 rounded-xl shadow-md border border-gray-200 relative">
        <div class="mb-4 flex flex-col gap-1">
          <h3 class="text-center text-2xl font-bold text-gray-600">{{ storeName }}</h3>
          <p v-if="storeAddress" class="text-center text-sm text-gray-600 mb-3"><span class="font-semibold">Alamat
              :</span> {{ storeAddress }}</p>
          <p class="text-sm text-gray-600"><span class="font-semibold">Tanggal :</span> {{
            formatDateIndo(transactionDate) }}</p>
          <p v-if="customerName" class="text-sm text-gray-600"><span class="font-semibold">Pelanggan :</span> {{
            customerName }}</p>
        </div>
        <table class="w-full text-sm border-t border-b border-gray-300">
          <thead class="bg-gray-100 text-gray-800">
            <tr>
              <th class="text-left p-2">Produk</th>
              <th class="text-center p-2">Jml</th>
              <th class="text-right p-2">Harga</th>
              <th class="text-right p-2">Total</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in products" :key="index" class="border-t border-gray-200">
              <td class="p-2 text-gray-800">{{ item.name }}</td>
              <td class="text-center p-2 text-gray-800">{{ item.qty }}</td>
              <td class="text-right p-2 text-gray-800">Rp {{ formatCurrency(item.price) }}</td>
              <td class="text-right p-2 text-gray-800">Rp {{ formatCurrency(item.qty * item.price) }}
              </td>
            </tr>
          </tbody>
        </table>

        <div class="text-right font-bold text-lg mt-4 text-blue-700 border-t pt-2 border-blue-200">
        </div>
        <div class="flex flex-wrap gap-2 mb-6">
          <div class="flex-1 flex flex-col items-end">
            <div class="text-right font-bold text-gray-600">
              Total: Rp {{ formatCurrency(grandTotal) }}
            </div>
            <div class="mt-2 text-right text-gray-600 font-semibold">Lunas</div>
          </div>
        </div>
        
        <div class="text-center text-gray-400 text-xs mt-4">
          Dibuat dengan Yonota – {{ storeName }}.</div>

      </div>
    </div>

    <!-- invoice preview etc. continues -->

    <footer
      class="pl-5 fixed bottom-0 left-0 w-full flex justify-between items-center py-3 bg-white dark:bg-gray-800 border-t border-gray-200 dark:border-gray-700 text-gray-600 dark:text-gray-400 text-sm shadow z-50">
      <div class="text-center">
        © 2025 <span class="text-blue-600 dark:text-blue-400 hover:underline">Irfannurf</span>, Yonota mudahkan
        reimburse mu
      </div>
      <div class="flex space-x-4 pr-4">
        <!-- YouTube Icon -->
        <a href="https://www.youtube.com/@RockyDrive" target="_blank"
          class="text-gray-600 dark:text-gray-400 hover:text-red-600">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
            <path
              d="M23.498 6.186a2.99 2.99 0 00-2.105-2.11C19.5 3.5 12 3.5 12 3.5s-7.5 0-9.393.576a2.99 2.99 0 00-2.105 2.11C0 8.08 0 12 0 12s0 3.92.502 5.814a2.99 2.99 0 002.105 2.11C4.5 20.5 12 20.5 12 20.5s7.5 0 9.393-.576a2.99 2.99 0 002.105-2.11C24 15.92 24 12 24 12s0-3.92-.502-5.814zM9.75 15.02V8.98L15.5 12l-5.75 3.02z" />
          </svg>
        </a>
        <!-- GitHub Icon -->
        <a href="https://github.com/irfannur" target="_blank"
          class="text-gray-600 dark:text-gray-400 hover:text-blue-600">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
            <path
              d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.207 11.387.6.113.793-.263.793-.587v-2.17c-3.338.725-4.037-1.613-4.037-1.613-.55-1.387-1.338-1.756-1.338-1.756-1.087-.75.088-.725.088-.725 1.2.088 1.837 1.238 1.837 1.238 1.075 1.837 2.825 1.3 3.513.988.113-.775.425-1.3.775-1.6-2.662-.3-5.462-1.337-5.462-5.962 0-1.312.475-2.387 1.237-3.237-.125-.3-.537-1.512.113-3.15 0 0 1.012-.325 3.3 1.237.962-.263 2-.4 3.037-.4 1.037 0 2.075.137 3.037.4 2.287-1.562 3.3-1.237 3.3-1.237.65 1.638.238 2.85.113 3.15.762.85 1.237 1.925 1.237 3.237 0 4.637-2.8 5.662-5.462 5.962.437.375.825 1.112.825 2.237v3.312c0 .325.2.7.8.587C20.563 21.8 24 17.3 24 12c0-6.63-5.37-12-12-12z" />
          </svg>
        </a>
        <!-- GitLab Icon -->
        <a href="https://gitlab.com/irfannur238" target="_blank"
          class="text-gray-600 dark:text-gray-400 hover:text-orange-600">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
            <path
              d="M22.453 12.934l-2.1-6.463c-.2-.6-.8-.988-1.4-.988-.6 0-1.2.388-1.4.988l-1.5 4.612H8.447l-1.5-4.612c-.2-.6-.8-.988-1.4-.988-.6 0-1.2.388-1.4.988l-2.1 6.463c-.2.6 0 1.238.5 1.625l9.05 6.825c.4.3 1 .3 1.4 0l9.05-6.825c.5-.387.7-1.025.5-1.625z" />
          </svg>
        </a>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import html2pdf from 'html2pdf.js'
import html2canvas from 'html2canvas'

const storeName = ref('Warung Makan Yono')
const storeAddress = ref('')
const transactionDate = ref(new Date().toISOString().substr(0, 10))
const customerName = ref('')

const formatDateIndo = (dateStr) => {
  if (!dateStr) return ''
  const bulan = [
    'Januari', 'Februari', 'Maret', 'April', 'Mei', 'Juni',
    'Juli', 'Agustus', 'September', 'Oktober', 'November', 'Desember'
  ]
  const [year, month, day] = dateStr.split('-')
  return `${day} ${bulan[parseInt(month, 10) - 1]} ${year}`
}
const products = ref([{ name: '', qty: '', price: '', priceDisplay: '' }])
const isDarkMode = ref(false)

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value
  localStorage.setItem('theme', isDarkMode.value ? 'dark' : 'light')
  document.documentElement.classList.toggle('dark', isDarkMode.value)
}

onMounted(() => {
  const saved = localStorage.getItem('invoiceData')
  if (saved) {
    const parsed = JSON.parse(saved)
    storeName.value = parsed.storeName
    storeAddress.value = parsed.storeAddress || ''
    customerName.value = parsed.customerName || ''
    transactionDate.value = parsed.transactionDate
    products.value = parsed.products.map(p => ({
      ...p,
      priceDisplay: formatCurrency(p.price)
    }))
  }
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme === 'dark' || !savedTheme) {
    isDarkMode.value = true
    document.documentElement.classList.add('dark')
  }
})

watch([storeName, storeAddress, transactionDate, products], () => {
  const data = {
    storeName: storeName.value,
    storeAddress: storeAddress.value,
    customerName: customerName.value,
    transactionDate: transactionDate.value,
    products: products.value.map(p => ({ name: p.name, qty: p.qty, price: p.price }))
  }
  localStorage.setItem('invoiceData', JSON.stringify(data))
}, { deep: true })

const addProduct = () => {
  products.value.push({ name: '', qty: '', price: '', priceDisplay: '' })
}

const removeProduct = (index) => {
  products.value.splice(index, 1)
}

const updatePrice = (index) => {
  const raw = products.value[index].priceDisplay.replace(/[^\d]/g, '')
  const value = parseInt(raw || '0')
  products.value[index].price = value
  products.value[index].priceDisplay = formatCurrency(value)
}

const grandTotal = computed(() => {
  return products.value.reduce((total, item) => total + item.qty * item.price, 0)
})

const formatCurrency = (value) => {
  return Number(value || 0).toLocaleString('id-ID')
}

const invoice = ref(null)
const generatePDF = () => {
  html2pdf().set({
    margin: 0.5,
    filename: `Invoice-${transactionDate.value}.pdf`,
    image: { type: 'jpeg', quality: 0.98 },
    html2canvas: { scale: 2 },
    jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
  }).from(invoice.value).save()
}

const generateImage = () => {
  html2canvas(invoice.value).then(canvas => {
    const link = document.createElement('a')
    link.download = `Invoice-${transactionDate.value}.png`
    link.href = canvas.toDataURL('image/png')
    link.click()
  })
}
</script>

<style scoped>
.input {
  @apply p-2 border border-gray-300 dark:border-gray-600 rounded-md w-full bg-white dark:bg-gray-700 text-gray-900 dark:text-gray-100 focus:outline-none focus:ring-2 focus:ring-blue-300;
}

.btn {
  @apply px-4 py-2 rounded-md font-semibold shadow-sm;
}

.btn-blue {
  @apply bg-blue-500 text-white hover:bg-blue-600;
}

.btn-green {
  @apply bg-green-500 text-white hover:bg-green-600;
}

.btn-yellow {
  @apply bg-yellow-500 text-white hover:bg-yellow-600;
}

.btn-gray {
  @apply bg-gray-300 text-gray-800 hover:bg-gray-400 dark:bg-gray-600 dark:text-white dark:hover:bg-gray-500;
}

.btn-red {
  @apply bg-red-500 text-white hover:bg-red-600 px-2 py-1 text-sm;
}
</style>
