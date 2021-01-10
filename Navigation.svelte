<script>
  import {
    Collapse,
    Navbar,
    NavbarToggler,
    NavbarBrand,
    Nav,
    NavItem,
    NavLink,
    UncontrolledDropdown,
    DropdownToggle,
    DropdownMenu,
    DropdownItem
  } from "sveltestrap";

  let isOpen = false;

  function handleUpdate(event) {
    isOpen = event.detail.isOpen;
  }
</script>
<style>
  .font {
    color: white;
  }
</style>

<Navbar color="ligth test" light expand="md">
  <NavbarBrand href="/"><div class="font">Super Duper Converter<div></NavbarBrand>
</Navbar>