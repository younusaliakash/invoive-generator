<script setup>
import { ref, reactive } from 'vue'
import {invoice1, invoice2} from './data/data'
const data = reactive({
  sender: 'John Doe',
  billTo: 'Donuld Duck',
  shipTo: 'Donuld Ducks Office Address',
  invoiceNumber: 'DD-016',
  date: '08/01/2023',
  dueDate: '08/05/2023',
  additionalNote: 'Not Applicable',
  items: [{ "description": "Frontend", "quantity": 23, "rate": 50, "amount": 1150 }, { "description": "Backend", "quantity": 40, "rate": 50, "amount": 2000 }, { "description": "Devops", "quantity": 10, "rate": 150, "amount": 1500 }],
  notes: 'Account Number: 0199283',
  terms: 'Send by wire transfer',
  subtotal: '',
  tax: '',
  total: '',
  balanceDue: ''
})

function addnewItem() {
  data.items.push({
    description: '',
    quantity: '0',
    rate: '0',
    amount: '0'
  })
}

function getSubTotal() {
  let total = 0
  data.items.forEach((item) => {
    total += item.amount
  })
  data.subtotal = total
  return total
}

function getTotalAfterTax() {
  const tax = data.subtotal * data.tax / 100
  data.total = data.subtotal + tax
  return data.total
}
</script>

<template>
  <section class="mx-auto container bg-white border border-gray-400 min-h-screen p-12">
    <div class="flex justify-between">
      <div class="flex flex-col space-y-5 w-1/2s">
        <div class=" ">
          <img class="w-40" src="https://www.shutterstock.com/image-vector/invoice-typographic-stamp-sign-badge-600w-1027820257.jpg" alt="">
        </div>
        <p class="mt-5">
          Sender
        </p>
        <textarea name="" id="" cols="30" rows="2" v-model="data.sender"></textarea>
        <div class="flex space-x-2">
          <div class="flex flex-col">
            <span>Bill to</span>
            <textarea name="" id="" cols="30" rows="2" v-model="data.billTo"></textarea>
          </div>
          <div class="flex flex-col">
            <span>Ship to</span>
            <textarea name="" id="" cols="30" rows="2" v-model="data.shipTo"></textarea>
          </div>
        </div>
      </div>
      <div class="flex flex-col w-1/2 items-end">
        <h1 class="mt-12 text-4xl uppercase text-right mb-5">Invoice</h1>
        <input class="w-[200px] text-right" type="text" placeholder="Invoice Number">
        <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
          <p>
            <span>Date</span>
            <input class="ml-2 w-[200px]" v-model="data.date">
          </p>
          <p>
            <span>Due Date</span>
            <input type="date" class="ml-2 w-[200px]" v-model="data.dueDate">
          </p>
          <p>
            <span>Additional Note</span>
            <input class="ml-2 w-[200px]" type="text" v-model="data.additionalNote">
          </p>
        </div>
      </div>
    </div>
    <div class="mt-20">
      <table class="table-auto w-full">
        <tr class="bg-gray-800 text-left text-white">
          <th class="p-2 pl-5 w-1/2">Item</th>
          <th class="p-2">Quantity</th>
          <th class="p-2">Rate</th>
          <th class="p-2 w-[200px] text-right pr-5">Amount</th>
        </tr>
        <tr v-for="(item, index) in data.items" :key="index">
          <td class="py-1">
            <input v-model="item['description']" class="w-full pl-5" type="text" placeholder="Description" />
          </td>
          <td class="">
            <input v-model="item['quantity']" class="w-full" type="number" placeholder="Quantity" />
          </td>
          <td class="">
            <input v-model="item['rate']" class="w-full" type="number" placeholder="Rate">
          </td>
          <td class="pr-5 py-1 text-right text-gray-800">
            $ {{ item['amount'] = item['quantity'] * item['rate'] }}
          </td>
        </tr>
      </table>
      <button class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="addnewItem()">
        Add More
      </button>
      <button class="ml-5 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="Object.assign(data,invoice1)">
        Load Invoice #1
      </button>
      <button class="ml-5 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="Object.assign(data,invoice2)">
        Load Invoice #2
      </button>
      <p>
        {{ data }}
      </p>
    </div>
    <div class="mt-[200px]">
      <div class="flex justify-between">
        <div class="flex flex-col space-y-5 w-1/2">
          <span>Notes</span>
          <textarea name="" id="" cols="30" rows="2" v-model="data.notes"></textarea>
          <span>Terms</span>
          <textarea name="" id="" cols="30" rows="2" v-model="data.terms"></textarea>
        </div>
        <div class="flex flex-col w-1/2 items-end">
          <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
            <p>
              <span>Subtotal</span>
              <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Subtotal" :value="getSubTotal()">
            </p>
            <p>
              <span>Tax</span>
              <input type="number" class="text-right w-[200px] ml-2" v-model="data.tax">
            </p>
            <p>
              <span>Total</span>
              <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Total" :value="getTotalAfterTax()">
            </p>
            <p>
              <span>Balace Due</span>
              <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0" placeholder="Balance">
            </p>
          </div>
        </div>
      </div>
    </div>

  </section>
</template>

<style scoped>
input,
textarea {
  border: 1px solid #ddd !important;
  padding: 5px;
}
</style>
