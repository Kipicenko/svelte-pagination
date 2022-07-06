<script>
    import { createEventDispatcher } from 'svelte';
    import {usePagination, DOTS} from "./usePagination";
    import Modal from "../Modal/Modal.svelte";
    import Arrow from "../../assets/arrow.svg";
    import Close from "../../assets/close.svg";
    export let currentPage;
    export let siblingCount = 1;
    export let pageSize;
    export let totalCount;
    let isOpen = false;
    let value = '';
    $: value = value.replace(/[^0-9]/g, '')
    $: totalPageCount = Math.ceil(totalCount / pageSize) || 1;
    $: totalPageNumbers = siblingCount + 5;
    $: arrayPagination = usePagination({
        currentPage,
        siblingCount,
        totalPageCount,
        totalPageNumbers
    })

    const dispatch = createEventDispatcher();
    const changePage = (page) => {
        if (page !== currentPage) {
            dispatch('change', page);
        }
    }

    const send = () => {
        if (value > totalPageCount) {
            changePage(totalPageCount)
        } else if (value < 1) {
            changePage(1)
        } else {
            changePage(Number(value))
        }
        value = ''
        isOpen = false
    }
</script>

<nav class="pagination">
    <ul class="pagination__container">
        <li class="pagination__item" class:disabled={currentPage === 1} on:click={() => changePage(currentPage - 1)}>
            <div class="arrow left">
                <Arrow width={16} height={16} class="left-arrow-pagination"/>
            </div>
        </li>
        {#each arrayPagination as paginate}
            {#if paginate === DOTS}
                <li class="pagination__item dots" on:click={() => isOpen = true}>&#8230;</li>
            {:else}
                <li class="pagination__item" class:active={currentPage === paginate} on:click={() => changePage(paginate)}>{paginate}</li>
            {/if}
        {/each}
        <li class="pagination__item" class:disabled={currentPage === totalPageCount} on:click={() => changePage(currentPage + 1)}>
            <div class="arrow right">
                <Arrow width={16} height={16} class="right-arrow-pagination"/>
            </div>
        </li>
    </ul>
    {#if isOpen}
    <Modal on:close={() => isOpen = false}>
        <div class="modal__content" on:click={(e) => e.stopPropagation()}>
            <div class="modal__content-close" on:click={() => isOpen = false}>
                <Close width={16} height={16} class="modal__content-close"/>
            </div>
            <h3 class="modal__content-title">Открыть страницу</h3>
            <input type="text" bind:value  placeholder="Введите страницу" class="modal__content-value">
            <div style="text-align: center;">
                <button on:click={send} class:btn-disabled={!value} class="modal__content-btn btn-disabled">Применить</button>
            </div>
        </div>
    </Modal>
    {/if}
</nav>

<style lang="scss">
    .pagination {
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;

      &__container {
        display: flex;
        align-items: center;
        justify-content: center;
      }

      &__item {
        cursor: pointer;
        width: 30px;
        height: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
      }
    }

    .arrow {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 100%;
    }

    .disabled {
      pointer-events: none;

      & :global(.left-arrow-pagination) {
        fill: #E4E3E3;
      }

      & :global(.right-arrow-pagination) {
        fill: #E4E3E3;
      }
    }

    .active {
      background-color: #E4E3E3;
    }

    :global(.left-arrow-pagination) {
      transform: rotate(-180deg);
    }

    .modal__content {
      position: relative;
      width: 400px;
      height: auto;
      background-color: white;
      border-radius: 4px;
      padding: 25px;

      &-close {
        display: flex;
        align-items: center;
        justify-content: center;
        position: absolute;
        top: 8px;
        right: 8px;
        cursor: pointer;
    }

      &-title {
        text-align: center;
        margin-bottom: 20px;
      }

      &-value {
        display: block;
        width: 100%;
        height: calc(2.25rem + 2px);
        padding: 0.375rem 0.75rem;
        font-family: inherit;
        font-size: 1rem;
        font-weight: 400;
        line-height: 1.5;
        color: #212529;
        background-color: #fff;
        background-clip: padding-box;
        border: 1px solid #bdbdbd;
        border-radius: 0.25rem;
        margin-bottom: 20px;
        transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;

        &::placeholder {
          color: #212529;
          opacity: 0.4;
        }

        &:focus {
          color: #212529;
          background-color: #fff;
          border-color: #bdbdbd;
          outline: 0;
          box-shadow: 0 0 0 0.2rem rgba(158, 158, 158, 0.25);
        }
      }

      &-btn {
        border: none;
        background-color: #69DB73;
        cursor: pointer;
        padding: 10px;
        color: white;
        transition: background-color 0.2s ease;

        &:hover {
          background-color: #57B55F;
        }
      }
    }
    .btn-disabled {
      pointer-events: none;
      background-color: #AEB5B2
    }
</style>