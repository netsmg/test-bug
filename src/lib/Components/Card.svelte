<div class="card">
    <slot></slot>
</div>

<style>
    .card {
        text-align: center;
        max-width: 900px;
        width: 180px;
        height: 430px;
        padding: 15px 45px 15px 50px;
        border-radius: 20px;
        background: #fff;
        box-shadow: 2px 8px 12px rgba(1, 109, 28, 0.336);

        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        /* linear-gradient(120deg, #17B169,#00693E); */
    }
</style>
