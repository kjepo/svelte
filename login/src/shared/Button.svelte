<script>
  // default button
  export let color = 'primary';
  export let type = 'submit';
  export let flat = false;
  export let inverse = false;
</script>

<button type={type} class:flat={flat} class:inverse={inverse} class={color} on:click>
  <slot></slot>
</button>

<style>
button {
    border: 0;
    cursor: pointer;
    border-radius: 6px;
    padding: 8px 12px;
    font-weight: bold;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.2);
}
.flat {
    box-shadow: none;
}
.primary {
    background: #0d6efd;
    color: white;
}
.secondary {
    background: #6c757d;
    color: white;
}
.success {
    background: #198754;
    color: white;
}
.danger {
    background: #dc3545;
    color: white;
}
.warning {
    background: #ffc107;
    color: #000;
}
.info {
    background: #0dcaf0;
    color: #000;
}
.light {
    background: #f8f9fa;
    color: #000;
}
.dark {
    background: #212529;
    color: white;
}
.link {
    background: transparent;
    color: #0d6efd;
    box-shadow: none;
    text-decoration: underline;
    padding: 0;
}
.link:hover {
    color: #0a58ca;
}
.primary.inverse {
    color: #0d6efd;
    background: white;
    border: 2px solid #0d6efd;
}
.secondary.inverse {
    color: #6c757d;
    background: white;
    border: 2px solid #6c757d;
}
.success.inverse {
    color: #198754;
    background: white;
    border: 2px solid #198754;
}
.danger.inverse {
    color: #dc3545;
    background: white;
    border: 2px solid #dc3545;
}
.warning.inverse {
    color: #ffc107;
    background: white;
    border: 2px solid #ffc107;
}
.info.inverse {
    color: #0dcaf0;
    background: white;
    border: 2px solid #0dcaf0;
}
.dark.inverse {
    color: #212529;
    background: white;
    border: 2px solid #212529;
}
</style>
