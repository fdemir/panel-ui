<script>
  import {isPageInitialized} from "../../Store"

  isPageInitialized.set(true);
</script>

<!-- All Players Page -->
<div class="content">
  <!-- All Players -->

  <div class="card">
    <div class="card-body">
      <div class="row justify-content-between">
        <div class="col-md-6 col-12 text-md-left text-center">
          <h5 class="card-title">
            50 Oyuncu -
            <span class="text-primary">1/5</span>
          </h5>
        </div>
        <div class="col-md-6 col-12 text-md-right text-center">
          <div class="btn-group">
            <router-link
                    class="btn btn-sm btn-outline-light btn-link"
                    role="button"
                    to="/panel/players/all"
            >
              Tümü
            </router-link>
            <router-link
                    class="btn btn-sm btn-outline-light btn-link"
                    role="button"
                    to="/panel/players/special"
            >
              Özel
            </router-link>
            <router-link
                    class="btn btn-sm btn-outline-light btn-link text-danger"
                    role="button"
                    to="/panel/players/banned"
            >
              Cezalı
            </router-link>
          </div>
        </div>
      </div>

      <!-- No Players -->
      <div class="container text-center d-block">
        <i aria-hidden="true" class="fas fa-users fa-4x text-glass m-3"></i>
        <p class="text-gray">Burada içerik yok.</p>
      </div>

      <!-- Posts Table -->
      <div class="table-responsive">
        <table class="table mb-0">
          <thead>
          <tr>
            <th scope="col"></th>
            <th scope="col">İsim</th>
            <th scope="col">Talepler</th>
            <th scope="col">Durum</th>
            <th scope="col">Son Oturum</th>
            <th scope="col">Kayıt</th>
          </tr>
          </thead>
          <tbody>
          <tr>
            <th class="min-w-50px" scope="row">
              <div class="dropdown position-absolute">
                <a
                        aria-expanded="false"
                        aria-haspopup="true"
                        class="icon-link d-block m-auto"
                        data-toggle="dropdown"
                        href="javascript:void(0);"
                        id="postAction"
                        title="Eylemler"
                >
                  <i aria-hidden="true" class="fa fa-ellipsis-v px-3"></i>
                </a>
                <div
                        aria-labelledby="postAction"
                        class="dropdown-menu dropdown-menu-right"
                >
                  <a class="dropdown-item" href="javascript:void(0);">
                    <i
                            aria-hidden="true"
                            class="fa fa-user-tag fa-fw text-primary"
                    ></i>
                    Yetkilendir
                  </a>
                  <a
                          class="dropdown-item"
                          data-target="#conformBanTickets"
                          data-toggle="modal"
                          href="javascript:void(0);"
                  >
                    <i
                            aria-hidden="true"
                            class="fa fa-times fa-fw text-danger"
                    ></i>
                    Taleplerini Yasakla
                  </a>
                </div>
              </div>
            </th>
            <td class="min-w-200px">
              <router-link title="Oyuncu Profiline Git" to="#">
                <img
                        alt="Oyuncu Adı"
                        class="rounded-circle border mr-3"
                        height="32"
                        src="https://minotar.net/avatar/e5eea5f735c444a28af9b2c867ade454/32"
                        width="32"
                />
                Kahverengi
              </router-link>
            </td>
            <td>
              0
              <i
                      aria-hidden="true"
                      class="fa fa-times text-danger fa-fw"
                      v-tooltip:top="'Talepleri Yasaklı'"
              ></i>
            </td>
            <td>
                <span
                        class="badge badge-pill badge-success text-white"
                        v-tooltip:top="'Sitede'"
                >
                  <i aria-hidden="true" class="fa fa-globe fa-fw"></i>
                  <span class="d-md-inline d-none">Çevrimiçi</span>
                </span>
            </td>
            <td>10 dakika önce</td>
            <td>01.01.2019</td>
          </tr>
          </tbody>
        </table>
      </div>

      <!-- Pagination -->
      <nav class="pt-3">
        <ul class="pagination pagination-sm mb-0 justify-content-start">
          <li class="page-item">
            <a
                    class="page-link"
                    href="javascript:void(0);"
                    title="Önceki Sayfa"
            >
              <span aria-hidden="true">&laquo;</span>
            </a>
          </li>

          <li class="page-item">
            <a class="page-link" href="javascript:void(0);">1</a>
          </li>

          <li class="page-item">
            <a
                    class="page-link"
                    href="javascript:void(0);"
                    title="Sonraki Sayfa"
            >
              <span aria-hidden="true">&raquo;</span>
            </a>
          </li>
        </ul>
      </nav>
    </div>
  </div>

  <!-- Confirm Ban User Tickets Modal -->
  <div
          aria-hidden="true"
          class="modal fade"
          id="conformBanTickets"
          role="dialog"
          tabindex="-1"
  >
    <div class="modal-dialog modal-dialog-centered" role="dialog">
      <div class="modal-content">
        <div class="modal-body text-center">
          <div class="pb-3">
            <i
                    aria-hidden="true"
                    class="fa fa-question-circle fa-4x d-block m-auto text-gray"
            ></i>
          </div>
          <span>
            Bu kullanıcının taleplerini gerçekten yasaklamak istiyor musunuz?
          </span>
        </div>
        <div class="modal-footer">
          <button
                  class="btn btn-outline-primary w-100"
                  data-dismiss="modal"
                  type="button"
          >
            Hayır
          </button>
          <button class="btn btn-danger w-100" type="button">
            Evet
          </button>
        </div>
      </div>
    </div>
  </div>
</div>