<script>
  import Icon from "svelte-awesome";
  import { faQuestionCircle } from "@fortawesome/free-solid-svg-icons";
</script>

<div
  aria-hidden="true"
  class="modal fade"
  id="ConfirmRemoveAllNotificationsModal"
  role="dialog"
  tabindex="-1">
  <div class="modal-dialog modal-dialog-centered" role="dialog">
    <div class="modal-content">
      <div class="modal-body text-center">
        <div class="pb-3">
          <Icon
            data="{faQuestionCircle}"
            scale="3"
            class="d-block m-auto text-gray" />
        </div>
        Tüm bildirimleri kalıcı olarak silmek istediğinizden emin misiniz?
      </div>
      <div class="modal-footer">
        <button
          class="btn btn-link text-muted"
          data-dismiss="modal"
          type="button">
          İptal
        </button>
        <button class="btn btn-danger" type="button">Evet</button>
      </div>
    </div>
  </div>
</div>
