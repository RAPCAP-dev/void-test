Тестовое задание: "Найди и исправь баги (React)"
Ориентировочное время: 15–25 минут
Стек: React 18+, функциональные компоненты, TypeScript, хуки
Задача
Дан компонент «Состав проекта» — интерфейс для формирования списка услуг. Пользователь выбирает услугу, указывает количество, добавляет в таблицу и видит итоговую стоимость.

Проблема: функциональность реализована, но есть несколько багов. Ваша задача — найти их, исправить и кратко описать, в чём была причина каждого бага.

Что нужно сделать
Скопируйте код в чистый React + TypeScript проект (Create React App, Vite или Next.js — на ваш выбор).
Запустите и воспроизведите баги (попробуйте добавить услуги, изменить количество, удалить строку).
Исправьте код с использованием хуков React.
В файле BUGFIX.md (или в комментариях в коде) кратко опишите каждый найденный баг: что ломалось и почему.
Формат сдачи
Ссылка на GitHub-репозиторий с исправленным кодом и файлом BUGFIX.md

Исходный код
import React from 'react'

interface Service {
id: number
name: string
price: number
}

interface ServiceItem {
service: Service
quantity: number
}

const availableServices: Service[] = [
{ id: 1, name: 'Разработка лендинга', price: 50000 },
{ id: 2, name: 'Интернет-магазин', price: 150000 },
{ id: 3, name: 'Корпоративный портал', price: 300000 },
{ id: 4, name: 'SEO-оптимизация', price: 20000 },
]

export default function ProjectBuilder() {
let selectedServices: ServiceItem[] = []
let selectedServiceId: number | string = ''
let quantity: number = 1

let totalPrice = 0
for (const item of selectedServices) {
totalPrice += item.service.price * item.quantity
}

function addService() {
const service = availableServices.find(s => s.id === selectedServiceId)
if (!service) return

const existing = selectedServices.find(item => item.service === service)
if (existing) {
existing.quantity += quantity
} else {
selectedServices.push({ service, quantity })
}
}

function removeService(item: ServiceItem) {
const index = selectedServices.indexOf(item)
if (index > -1) {
selectedServices.splice(index, 1)
}
}

return (
<div className="project-builder" style={{ maxWidth: 700, margin: '0 auto', fontFamily: 'sans-serif' }}>
<h2>Состав проекта</h2>

<div className="add-service" style={{ display: 'flex', gap: 12, marginBottom: 24, alignItems: 'center' }}>
<select
value={selectedServiceId}
onChange={e => { selectedServiceId = Number(e.target.value) }}
>
<option value="" disabled>Выберите услугу</option>
{availableServices.map(service => (
<option key={service.id} value={service.id}>
{service.name} — {service.price.toLocaleString()} ₽
</option>
))}
</select>

<input
type="number"
min={1}
max={10}
value={quantity}
onChange={e => { quantity = Number(e.target.value) }}
style={{ width: 80, padding: '8px 12px' }}
/>

<button onClick={addService} disabled={!selectedServiceId}>
Добавить
</button>
</div>

{selectedServices.length > 0 ? (
<table className="services-table" style={{ width: '100%', borderCollapse: 'collapse' }}>
<thead>
<tr>
<th style={{ border: '1px solid #ddd', padding: 10, textAlign: 'left' }}>Услуга</th>
<th style={{ border: '1px solid #ddd', padding: 10, textAlign: 'left' }}>Цена за ед.</th>
<th style={{ border: '1px solid #ddd', padding: 10, textAlign: 'left' }}>Кол-во</th>
<th style={{ border: '1px solid #ddd', padding: 10, textAlign: 'left' }}>Сумма</th>
<th style={{ border: '1px solid #ddd', padding: 10, textAlign: 'left' }}></th>
</tr>
</thead>
<tbody>
{selectedServices.map(item => (
<tr key={item.service.id}>
<td style={{ border: '1px solid #ddd', padding: 10 }}>{item.service.name}</td>
<td style={{ border: '1px solid #ddd', padding: 10 }}>{item.service.price.toLocaleString()} ₽</td>
<td style={{ border: '1px solid #ddd', padding: 10 }}>{item.quantity}</td>
<td style={{ border: '1px solid #ddd', padding: 10 }}>
{(item.service.price * item.quantity).toLocaleString()} ₽
</td>
<td style={{ border: '1px solid #ddd', padding: 10 }}>
<button onClick={() => removeService(item)}>Удалить</button>
</td>
</tr>
))}
</tbody>
<tfoot>
<tr>
<td colSpan={3} style={{ border: '1px solid #ddd', padding: 10 }}>
<strong>Итого:</strong>
</td>
<td colSpan={2} style={{ border: '1px solid #ddd', padding: 10 }}>
<strong>{totalPrice.toLocaleString()} ₽</strong>
</td>
</tr>
</tfoot>
</table>
) : (
<p style={{ color: '#888', fontStyle: 'italic' }}>Пока не добавлено ни одной услуги</p>
)}
</div>
)
}